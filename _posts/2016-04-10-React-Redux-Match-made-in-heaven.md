---
layout: post
title: 'React & Redux - Match made in heaven?'
date: 2016-04-10 21:03:15
author: chris
desc: My introduction to React & Redux
tags: [Web Development, JavaScript, React, Redux]
categories: [Technical]
image: assets/images/2016-04-10-React-Redux-Match-made-in-heaven/header.png
---

In this article, I will discuss my recent introduction to Facebook's [React](https://facebook.github.io/react/index.html). What I liked about it,  what I didn't and how [Redux](http://redux.js.org/) came to the rescue.

<!--more-->

Recently I needed to do a small web UI project and I decided to refresh my knowledge of JavaScript technologies and frameworks. I noticed that a lot of people were using React so I decided to give it a go. Until recently my go-to UI framework had been AngularJS and I had grown quite comfortable with it. So I was interested to see what React brings to the table.

As soon as I started reading about React, I was interested in a number of features it offers:
- It is very view focused and attempts to be agnostic of the rest of the stack
- It is designed to be fast (Virtual DOM, One way data-flow, etc.)
- Allows for easier modularity and re-use

The moment I started experimenting with it, I was not so sure anymore. The idea of JSX and having what to my eyes were parts of the template within my JavaScript code seemed weird. However, as I delved deeper I really started to like the React approach. Specifically, I really liked the structured approach that React imposes on you. It allows you to think of building a UI in the same way you would think about splitting a project into differnet classes in a higher level object-oriented language. Moreover, it lends itself very much to building your UI via Evolutionary Prototyping, of which I am a big fan. If you haven't done so already, I would strongly suggest going over the [Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html) article in the React documentation as it paints the picture I am describing very clearly.

As I was following the [Comment Box tutorial](https://facebook.github.io/react/docs/tutorial.html) from the Redux documentation, I felt unsure once again. Whilst the use of static propTypes made perfect sense to me, I did not really like the code once React state was introduced. To my eyes, the breaking down of the Combo Box component into many classes (or subcomponents), which till just now I had loved, suddenly ended up being a double-edged sword because it was causing state manipulation to be spread over many classes as well. The code was still cohesive because each subcomponent would only manipulate its own state. However, whereas before the interface points between subcomponents were very clean, now they did not tell the full story because of the internal state manipulation. The Comment Box component still looked elegant from the outside, but from the inside, not so much.

At that point, I read about how a lot of people that use React also use another technology called Redux. The Redux team themselves openly state that although React and Redux and not related to each other, and Redux can be used with a variety of other frameworks, they do fit very well together and Redux team actually provide an integration library called *react-redux* to make integration even easier.

Redux calls itself a *state management library*. State is maintained in a *state tree* that is contained in a *store*. The most interesting aspect is that state can only be manipulated by emitting an *action*. Redux then employs pure *reducers* that manipulate the state tree based on the action object that describes what happened.

This seemed like a perfect solution for my concern about having dispersed state manipulation in React code and I immediately got a sense of why so many people pair these two technologies together.

My objective at this point was to modify the [React Comment Box tutorial](https://facebook.github.io/react/docs/tutorial.html) which I had just completed and leverage Redux to further clean-up my code. This ended up being not as straight forward as I had hoped. No doubt due to my being completely new to both React and Redux.

Eventually, after a lot of reading and a lot of trial and error, I did manage to achieve my target, the sources for which are available [here](https://github.com/powell-christopher/react-tutorial-using-brunch-react-redux).

My main problem was that a lot of the material I found online was assuming a bit too much existing knowledge of these libraries. Knowledge which I was lacking. Also, I struggled to find a really simple explanation of how React and Redux fit together and what each library is trying to do.

In my next blog post, I will go over how I managed to introduce Redux to the React Combo Box tutorial provided by Facebook. I will explain how this allowed me to considerably clean-up my code and make it much more readable and maintainable. I will also explain the basics of how React and Redux interact with each other to achieve the desired result.