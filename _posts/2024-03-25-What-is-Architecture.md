---
layout: post
title: What is Architecture?
date: 2024-03-25 20:00:00
author: chris
categories: [Technical]
image: assets/images/2024-03-25-What-is-Architecture/header.png
tags: [featured, Architecture, Communication, Leadership]
---
We all understand that good communication is critical to any productive collaboration. We also understand the need for shared language or shared understanding. This is why, in an interview context, once we get beyond the introduction stage, one of the first questions I ask is 'What is Architecture?', or 'What is the Architect Role?'. These are not gotcha questions. There exists a divergence of understanding of what the role involves, largely based on first-hand experience and local context. Thus I find these questions to be good ice-breakers to see how closely a candidate and myself align on our understanding of the role and its attached responsibilities as that is what a possible collaboration would be based on.

What is Architecture?
=====================

Even Martin Fowler in his seminal whitepaper "[Who Needs an Architect?](https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf)" refused to attempt to give a definition. 

He instead relied on Ralph Johnson's famous definition from a post on the Extreme Programming mailing list:

> "Architecture is about the important stuff. Whatever that is." - Ralph Johnson

A similarly abstract definition comes from Edwin Marrima below: 

> "[Architecture is the stuff you can't Google.](https://www.linkedin.com/pulse/architecture-stuff-you-cant-google-edwin-marrima/)" - Edwin Marrima

The above definitions, while perhaps a bit 'tongue-in-cheek' have some hidden nuance. 

Architecture being about what is important alludes to the fact that architecture should focus on the important questions. What gives the most value or what is most critical or costly. 

The idea that architecture is the stuff you cannot Google alludes to the fact that architecture deals with highly dynamic and highly contextual problems, as opposed to highly specific problems that others will have encountered and solved, thus solving them for everybody else. 

This also elucidates some of the reasons why coming up with a good definition is such a challenge.

> "[Software architecture is about making fundamental structural choices that are costly to change once implemented.](https://en.wikipedia.org/wiki/Software_architecture)" - Wikipedia

The above is probably more in-line with a typical answer from someone trying to offer a more concrete definition. However, any such 'comprehensive' definition will quickly becomes out-of-date due to the dynamic nature of architecture. Is architecture exclusively concerned with fundamental structural choices? Does that hold in today's industry with say a DevOps based project/company? What is the real cost of 'costly changes' when costs can be offset through technology? Should we be so scared of change when working iteratively making change a first-class-citizen within one's process?

Yet another definition comes from [TOGAF](https://pubs.opengroup.org/architecture/togaf9-doc/arch/chap02.html#:~:text=ISO/IEC/IEEE,evolution%20over%20time.%22), which is actually two-fold, based on context:

<blockquote>
"The fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution."
<br><br>
"The structure of components, their inter-relationships, and the principles and guidelines governing their design and evolution over time."

    - TOGAF Standard 9.2
</blockquote>

So now we have the idea of 'design principles' coming through. Clearly, describing or taking decisions relative to an element's structure is not sufficient and we need to discuss the governing principles on which it is based.

Finally, here is what is offered by [ISO/IEC/IEEE 42010:2022 - Software, systems and enterprise
Architecture description](https://cwe.ccsds.org/sea/docs/SEA-SA/Background%20Materials/ISO%20IEC%20IEEE%2042010%202022.pdf):

<blockquote>
"An architecture of an entity, expressed in one or more architecture descriptions (AD), assists in understanding the fundamental concepts or properties of the entity, pertaining to its structure, behavior, design and evolution, such as feasibility, utility and maintainability and fundamental concepts for its development, operation, employment, external impacts, utilization and decommissioning." 
    
    - ISO/IEC/IEEE 42010:2022
</blockquote>

The above is certainly thorough, but not exactly concise. Also, the dynamic nature of architecture coupled with the thoroughness of the above definition makes it brittle; meaning it will need to be constantly updated as the already long list of duties/responsibilities/concerns listed above continues to shift and expand over time.

I will not attempt to provide my own definition, knowing that it will be incomplete, probably contextual to my current experience, and even if it were to be correct, would quickly become out-of-date as architecture continues to evolve. 

Perhaps we can start by agreeing on some possible tenets:
- No agreed upon concise definition
- Massive in scope
- Continuously changing/expanding
- Operates in a rapidly evolving ecosystem
- Dynamic in nature
- Highly contextual
- Everything has a trade-off

Based on these tenets, we can move on to describing what our expectations are for someone filling the architecture role. What responsibilities are attached to this role?

What is the Role of an Architect?   
=================================

While discussing the architecture role, one encounters an equally varied list of responses as to when trying to define architecture itself.

The role ranges from what one could consider a highly senior programmer, to someone who designs applications or systems, to someone that translates business requirements into technical specifications, to someone that champions communication between the technical and non-technical, all the way to someone that defines high level technical direction for the company. This is further compounded by whether the company has adopted agile or not, and how the architect would fit into the development lifecycle.

Of course there is nuance here depending on what type of architect we are talking about: a technical architect? a solutions architect? an enterprise architect?

Nevertheless, there is a set of expectations that are true for all types of architects. This is something that I learned myself through education and first hand experience over my years in architecture. I recently encountered a very good concise definition of these expectations in [Fundamentals of Software Architecture: An Engineering Approach](https://www.thoughtworks.com/insights/books/fundamentals-of-software-architecture) by Mark Richards and Neil Ford, a book I wholeheartedly recommend.

In the chapter 1, they propose the following expectations for an architect irrespective of role, title or job description:
- Make architecture decisions
- Continually analyze the architecture
- Keep current with latest trends
- Ensure compliance with decisions
- Diverse exposure and experience
- Have business domain knowledge
- Possess interpersonal skills
- Understand and navigate politics

This neatly aligns with my own belief of what an effective architect should do. Let's elaborate a bit on the above...

- An architect is expected to define the architecture decisions and design principles used to guide technology decisions within the team, the department, or across the company.
- An architect is expected to continually analyze the architecture and current technology environment and then recommend solutions for improvement.
  - We are dealing with a dynamic ecosystem which the architect needs to holistically analyze in order to determine architecture vitality and protect against architectural decay.
- An architect is expected to keep current with latest technology and industry trends.
  - A developer needs to keep current with latest technology. An architect even more so as his decisions are longer-lasting and more impactful.
- An architect is expected to ensure compliance with architecture decisions and design principles.
  - Lack of compliance is another source of architecture decay which can result in the system not meeting its intended architecture characteristics.
- An architect is expected to have exposure to multiple and diverse technologies, frameworks, platforms and environments.
  - For an architect, technical breath is more critical than technical depth, although both are necessary. It is more valuable for an architect to be aware of multiple technologies/frameworks/platforms/etc., thus providing multiple options to be analyzed and increasing our chances of success. Of course being an expert in a particular technology/framework/platform/etc. is also beneficial especially if it is something that is of value to the company.
- An architect is expected to have a certain level of business domain expertise.
  - Without domain knowledge, it is difficult to understand the business problem, goals and requirements, making it difficult to design an effective architecture to meet the requirements of the business.
  - Domain knowledge supports effective communication which increases confidence.
  - Domain knowledge is required to identify implicit requirements.
- An architect is expected to possess interpersonal skills, including teamwork, facilitation, and leadership.
  - As technologists, this can be challenging as we more inclined to dealing with technical problems than people problems.
  - <blockquote> "No matter what they tell you, it is always a people problem." - Gerald Weinberg, computer scientist, author and teacher of the psychology and anthropology of computer software development.</blockquote>
- An architect is expected to understand the political climate of the enterprise and be able to navigate the politics.
  - This can very frustrating to junior and established architects alike. The reality is that as the impact of our decisions grow in scale, the group of impacted people grows and with that so does the number of challenges.

I believe being cognisant of, practicing, and working on improving at these disciplines is  critical to success in architecture.

Speaking of success, I would also strong recommend the previously mentioned book: [Fundamentals of Software Architecture: An Engineering Approach](https://www.thoughtworks.com/insights/books/fundamentals-of-software-architecture) by Mark Richards and Neil Ford. It is an excellent book I would highly recommend to everyone, from people interesting in a career in architecture wanting to better understand the role, to junior architects wanting to get a more solid grasp on how they improve, to established architects as a way of better crystallizing their existing knowledge and experience.

> This post takes inspiration from Chapter 1 of [Fundamentals of Software Architecture: An Engineering Approach](https://www.thoughtworks.com/insights/books/fundamentals-of-software-architecture) by Mark Richards and Neil Ford.