---
layout: post
title: Getting Started with React-Redux
date: 2016-04-10 21:29:02
author: chris
desc: A basic introduction to React-Redux
tags: [Web Development, JavaScript, ES6, React, Redux, React-Redux, Brunch, node.js]
categories: [Technical]
image: assets/images/2016-04-10-Getting-Started-with-React-Redux/header.png
---


In this article, we will dig through the details of my re-implementation of FaceBook's React POC. I will go over the process of implementing this version of the POC, areas of importance, lessons learned, etc.

The source code is available [here](https://github.com/powell-christopher/react-tutorial-using-brunch-react-redux).

<!--more-->

Continuing on from my previous blog post, my objective for this POC was to re-implement React's [Comment Box tutorial](https://facebook.github.io/react/docs/tutorial.html) using Redux as a replacement for React State. If you have not yet gone through the tutorial, I would highly recommend that you do so as it is highly informative and I will be assuming that prior knowledge. My implementation will be based on the static implementation of that tutorial.

First things first...I wanted a scaffolding tool to get me started with my node based project. I was a bit rusty on node scaffolding tooling having only really had experience with [Yoeman](http://yeoman.io/) in the past. So I decided to have a look around, see what's new and experiment a bit. In doing so, I discovered [Brunch](http://brunch.io/). Brunch is actually one of the oldest players in the scaffolding game. What I liked about it is its simplicity. 

If you already have node.js installed, installation is a doddle:

```
npm install -g brunch
```
{: data-title="Brunch.io setup"}

Once brunch is installed, it offers a CLI that allows you to get going very easily. Ultimately the CLI will create JSON based configuration which, should you choose to, can also be easily manipulated directly.

What I really liked about it though is the concept of *skeletons*. A *skeleton* is basically just a boiler plate project. If you decide to build your project based on a *skeleton*, brunch will just download that *skeleton* for you, customize it based on your parameters, and off you go. All required dependencies are included out of the box. No messing around. It just works. Obviously, such a mechanism is not as powerful or flexible as what other frameworks offer, but Brunch has an extensive list of *skeletons* on offer. Chances are they have exactly what you're looking for. In my case, I picked the [React+Redux Skeleton](https://github.com/brunch/with-redux) and it was a great starting point.

The project is laid out as follows:
* actions
* assets
* components
* drivers
* reducers
* styles
* initialize.js

Lets start breaking this down...

The *assets* folder contains my web assets. All I have there is my *index.html* file. Our starting point. This HTML page just acts as a container for *initialize.js*, which is where things really start kicking off. This is where we find our *ReactDom.render()* call to which we pass JSX that pulls in a couple of components, namely *WelcomeMessage* and *CommentBox*.

Our components can be found in the *components* folder. *WelcomeMessage* is a purely static component. As simple as it gets. Just a *render* call with some JSX inside. *CommentBox* is a bit more elaborate but it is largely based on the React Tutorial. Once again we have a *render* method with some JSX which this time includes the *CommentList* and *CommentForm* components. *CommentList* expects a map of comments to render using the *Comment* component, whereas *CommentForm* expects a function to be called on Form Submit. 

So far, so good, so static...

For now, we have a nice logical segregation of our UI components and little else. If we continue on the footsteps of the original React tutorial, this is where we would introduce React State. Unfortunately that has the effect of introducing *setState* and *getState* methods all over our components making readability and maintainability suffer.

This is where [Redux](http://redux.js.org/) comes in...

So what is Redux? To quote their website, it is a "predictable state container for JavaScript apps". In Redux, the state of the whole application is maintained in a single *store* as an object tree. The only way the store can be modified is through the emitting of an *action*. The *action* is handled by a *reducer* which will itself modify the *store*. All this should sound pretty standard to anyone used to event based systems in higher level languages. It was however incredibly exciting for me to find this same paradigm in the JavaScript world.

So how does Redux fit into our tutorial code? Very neatly it turns out :) The Redux team have even seen fit to create a library for the easy integration of React and Redux, intuitively named *react-redux*. Let's continue...

Going back to *initialize.js*, we import *redux* and *react-redux*. We then create our store via the *createStore* command. We're creating the store here at the very topmost layer since as previously mentioned, we will have a single store maintaining the state of the whole application. Next we modify our JSX and we wrap our *CommentBox* tag with a *Provider* tag which accepts our store object as a parameter.

```javascript
const store = createStore(commentsReducer);
```
{: data-title="Store setup"}


```javascript
<Provider store={ store }>
	<CommentBox url="/api/comments" pollInterval={2000} />
</Provider>
```
{: data-title="Provider setup"}

The *Provider* tag comes from *react-redux*. It will make the Redux *store* available within the context of all React based components that are wrapped by the *Provider* tag, including any other components that are pulled in further down the chain of JSX based React *render* methods. At this point we can pull our React Props from the Redux store via a *store.getState().PROP_NAME* call. Note that if we have a look at *CommentBox*, this is being done within the *render* method and the Props are handled as constants just like any standard React Prop. Another thing to notice is that all the changes required for the introduction of the Redux Store into the application are isolated within *initialize.js* and *CommentBox* (my top level component). The rest of the application is unaffected. This allows for a nice evolutionary transition from a static React Prop based application to a dynamic, event-based application.

```javascript
const { store } = this.context;
const { getState } = store;
const { comments } = getState();
```
{: data-title="Using the store"}


Now that we have our Redux integration, we need to start manipulating the store via *actions* and *reducers*.

Starting with *actions*, I created *CommentActions* and thought "Which actions or events does my application need to handle?". I thus created my two actions, SUBMIT_COMMENT and RECEIVE_COMMENTS. I then proceeded to create a function representing each one. The relevant function will have as parameter the intended payload for the action and it will return an object which defines one of the actions we just created as its *type* and a data payload. In my case, the payload passed to the action is the same one the action passes along as its payload. However, one could have some business logic in here which modifies the payload somehow.

```javascript
// redux actions defined as const of type Symbol
export const SUBMIT_COMMENT   = Symbol('SUBMIT_COMMENT');
export const RECEIVE_COMMENTS = Symbol('RECEIVE_COMMENTS');

/**
  submitComment
  Defines payload for events of type SUBMIT_COMMENT
**/
export function submitComment(comment) {
  return {
    type: SUBMIT_COMMENT,
    comment
  };
}

/**
	receiveComments
	Defines payload for events of type RECEIVE_COMMENTS
**/
export function receiveComments(comments) {
  return {
    type: RECEIVE_COMMENTS,
    comments
  };
}
```
{: data-title="Actions"}

Now that we have *actions*, we need *reducers* to handle them. The *reducer* is responsible for altering the *store* based on the received *action*. In this case, we only have two *actions* to handle, so to keep things simple, we just  have a single *reducer* function which handles both via a switch statement. The *reducer* function will accept the current *store* and the *action* as parameters. It will then return the new *store*, which may or may not be identical to the previous version. In the case of SUBMIT_COMMENT we concat the new comment from the *action* payload to the current list of comments and then pass that along. For RECEIVE_COMMENTS we just pass the *action* payload out as the new list of comments. Notice that I also have a *default* option in the switch statement which just returns the current *store* as is. This is in case our *reducer* somehow receives an unhandled *action*. Notice that the *reducer* we have created is agnostic of the mechanism through which the *action* payload is obtained (i.e. the loading and submitting of comments on the server). The *reducer* only deals with the manipulation of the *store*.

```javascript
/**
  CommentReducers
  A Redux Reducer that can handle Comment Actions.
  Given an existing State Tree and an Action, it will respond with a new State Tree.
**/

import * as commentActions from 'actions/CommentActions';

export default function commentsReducer(state={comments : []}, action) {
  switch(action.type){
    case commentActions.SUBMIT_COMMENT:
      let comments = state.comments.concat([action.comment]);
      return {comments : comments};
    case commentActions.RECEIVE_COMMENTS:
     return {comments : action.comments};
    default:
      return state;
  }
}
```
{: data-title="Reducers"}

Now that we have *actions* and *reducers* in place, lets put them to use. Going back to *CommentBox* we will use the *store* object which was made available to us by *react-redux*`s *Provider* tag to dispatch *actions* via the *dispatch* function. This takes as parameter the response from the *action* function we created earlier in *CommentActions*. Note however that this function in turn requires the *action* payload as a parameter, meaning it is now time to reach out to the server. However, we do not want to dirty our *CommentBox* component with the details of server interaction, so instead we create *CommentsDriver* which will be the sole entity responsible for this interaction. So *CommentBox* will obtain the current data through the use of *CommentDriver* and then dispatch *actions* against the Redux *store* which will be handled by our *reducer*, which in turn will manipulate the *store*, which is then fed back to our React components via *react-redux*.   

```javascript
commentsAPI.fetchComments(this.props.url)
	.then((comments) => {
		console.log(comments);
        store.dispatch( commentActions.receiveComments(comments) );
	}).catch((error) => {
		console.error(error);
	});
```
{: data-title="Dispatching actions from CommentBox.js"}

An important thing to note, and something that escaped me at first, is that React components need to *subscribe* to *store* updates in order for the React context to be updated when the *store* changes. If you fail to do this, you will see *actions* being dispatched, *reducers* doing their job, but no visual change to the application. This is due to the fact that as a performance optimization, the React context will *not* be updated automatically unless the component specifically asks for the updates through a *subscribe* call. It is also good practice to *unsubscribe* before unloading a component. Also note that since our Redux *store* integration was confined to our top level *CommentBox* component, the *subscribe* and *unsubscribe* calls also only need to happen here. 

```javascript
  // componentDidMount is called automatically by React on mount of the component
  // here we subscribe to redux store (to get updates) and we load comments from server
  componentDidMount() {
    console.log('subscribing to redux store');
    const { store } = this.context;
    this.unsubscribe = store.subscribe( () => this.forceUpdate() );
    
    console.log('bootstrapping comment list');
    this.loadComments();
    setInterval(() => this.loadComments(), this.props.pollInterval);
  }
  
  // componentWillUnmount is called automatically by React on unmount of the component
  // here we cleanup by unsubscribing from the redux store
  componentWillUnmount() {
    console.log('unsubscribing from redux store');
    this.unsubscribe(); 
  }
```
{: data-title="Dispatching actions from CommentBox.js"}

And there you have it...our application is now fully dynamic and event driven. Nice! :)

In summary, I have found this approach to have a lot of advantages:
* React is giving us high modularity and re-usability of our components
* State is handled entirely by the Redux *store*
* We have an event driven application thanks to the use of *actions* and *reducers*
* Our *reducers* are only involved in the manipulation of the state *store* and nothing else
* All state *store* manipulation is localized within our *reducers*
* Both our React components and our Redux code are completely agnostic of the details of server interaction thanks to our *drivers*
* We have much better segregation of concerns then the dispersal of React State *getState* and *setState* throughput our components
* We moved from a static React Prop based application to a dynamic event driven application organically

Of course this small POC can be further improved using a variety of related tooling. One example is the use of [redux-thunk](https://github.com/gaearon/redux-thunk) in order to have *actions* return functions allowing for the delayed dispatch of *actions*, the introduction of business logic to the *action*, asynchronous dispatch of *actions*, etc. My scope for this post however was to demonstrate a basic integration of React and Redux and clearly explain the players and processes involved. As I learn more about these technologies I may decide to revisit this and share my findings once more :)

In the meantime, I hope you've enjoyed this little delve into React and Redux and hope you'll give then a go.

Till next time...