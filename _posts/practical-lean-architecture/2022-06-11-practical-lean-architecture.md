---
title: Practical Lean Architecture 
date: 2022-06-11 09:33:15 +05:30
tags: [ architecture]
description: Lean Architecture for agile projects.
image: "/practical-lean-architecture/pisa.jpg"
comment: true
---
<figure>
	<img src="pisa.jpg" alt="Leaning Tower"> 
	<figcaption style="color: grey !important;"> 
		Photo by <a href="https://unsplash.com/@spantax" style="color: grey !important;" target="_blank">Marco Ceschi</a> 
	</figcaption>
</figure>

Every software team now use Agile Software Development to some degree, Waterfall development is almost extinct. But the tension between software architecture and agile method remains. There is tensions between spending too little time designing an up-front architecture, increasing risk, and spending too much time, negatively impacting of the delivery of value to the customer. Despite various theoretical proposal confusion and tension exists in most of the teams with respect to Architecture. If you do [Google Search for Lean Architecture](https://www.google.com/search?q=lean+architecture){:target="_blank"}, content in all the links is rather abstract and confusing. 


Lean and Architecture seems contradictory. Developers sometime mistake agile for absense of architecture. Architecture is a conceptual model of a system and it can not be ignored. Altough the process and people and tools involved in Architecture process are different in Agile but the Software Architecure remains essential for good system. Having said that we can't have Ivory Tower Architecture rather we need to have requirment based architecture and the process will differ from team to team, but underlying philosophy of "Value Creation, Waste Reduction and Rapid Feedback" must be followed in Architecture proces.

Let's take a step back to understand  the word "Architecture", when we understand the essesnce of Architecure, we may be able to define lean methods. Software architecutre has it's roots in the physical Architecture. Roman Architecture was defined by three qualities:
1. __Utilitas__ (It must provide utility) 
2. __Firmitas:__ (It must be durable)
3. __Venustas:__ (It must inspire human senses) 

Lets's also have a look at lean principals:
1. Eliminate waste
2. Amplify learning
3. Decide as late as possible
4. Deliver as fast as possible
5. Empower the team
6. Build integrity in
7. Optimize the whole

We must also review the [Agile Manifesto](https://agilemanifesto.org/principles.html){:target="_blank"} before going further. It is very clear that above three are not disjoing sets, they do have an intersection and a rather large one. Lean Architecture is hard, it requires balance and there is silver bullet to get to the answer.But this intersecion may be starting point for understanding lean architecutre. If your design is lean, it produces an architecuture that can help be more agile. Let's try and understand Lean Architecture, through set of questions:


## Who is involved?

> In lean architecture, we lean towards people aspect of architecture a lot more than traditional software architecture. 

We must realzie that Architecure is not just about system, it is about people and it is more about people than about system. One of the cornerstone of lean architecture is "All Hands on Deck". The traditional architecutre was done in isolation by Architects in isolation, lean architecture involves all stakeholders from the word go. Here are the key stakeholdes:
- End User of System
- Business & Domain Experts
- Developers & Testers
- IT and Support

Also the decisions are not taken by Architects but architect facilitates the decision therough her/his knowlege and influence. They build bridges between different teams and empower developers make technology choices, Empower users to unlock expectations etc. They also need to put constraint on each category of user to be able to express clearly and concisely. 


## Who is Responsible for Architecture?
> The best architectures, requirements, and designs emerge from self-organizing teams.

As mentioned in Agile Manifesto, decisions are made by the the team collectivly and it would be naive to assume that the team will always be in agreement. Someone needs to lead/facilitate the team with regards to the evolution of the architecture. There is a need for Architecture owners. Very often the person in the role of team lead is also the architecture owner. This isn't always the case, particularly at scale, but it is very common for smaller agile teams. This person would need to have both the skills of an AO as well as a team lead to be successful doing so. More complex teams would have dedicated architecture owners. Here are responsibilities of Architure ownwers:
- Facilitate creation of the architecture, not create and enforce it. 
- Coach other team members in architecture skills and thinking. 
- Advise the product owner in technical priorities.
- Build architectural spikes.


## What is the scope of Architecture?
> Just Barely Good Enough Is Actually The Most Effective Architecture.

Every system has an architecture and every reasonable system needs an architecure. The process may differ, people involved may be more and role of architect might be different but that does not change the need for a sound architecture. The architecture must be based on requirment. This may sound obvious, but it is first step in wasting architectural waste. Architecture represents the significant design decisions that shape a system, where significant is measured by cost of change. Here are some guiding principle to define scope:
- *Architecture is about form, not structure*. 
- *Architecture is expression, not abstraction*
- *Architecture should define what system does and no more*. 

Architecture should provide a consistent system view, inconsistency increases waste. Scope of architecture is same as in traditional systems minus waste. Architecure is a continious process and some decisions are made just in time, this often requires fundamental change in mindset and team culture. Some high-level architectural modeling is doen early in the lifecycle to foster a common  technical strategy within the team and critical stakeholders. The goal at this point is to identify an architectural strategy, not write mounds of documentation. 


## Where do you get started?
> Problem definition is a clearly written statement of the problem. This clearly must be first step towards solving it!

Before we lay our route to destination, we shold know our desination. We must start with a clear problem definition. There is no surprise here, like any other software development process you start by modelling the problem. This is the foundation of the architecture. If we don't have a problem, why waste any resources? The problem modelling gives a sense of purpose and acts a guide to future decisions. 

The problem modelling must incorporate continuous innovation to increase value to end user. We have to model the problem that it is clearly defined and has flexibility as well. The problem definition should also become foundation for consistency and hence reducing waste. A well written problem statement offers a consistent version of direction and vision that defins precise driection but not the path. The problem definition may evolve during the project duration, but we should still strive to build the best possible first version. This is an up front investment, team comes together and defines and discovers the direction. We must strive to get to the root of the problem, in order to define it clearly. A Good Problm Definition :
1. is written Clearly and Concisely 
2. is difference between current and future state
3. has a measurable success criteria
4. is internally consistent.
5. is not defining a solution

## How much Documentation?
> If we aim to ship software every sprint, our documentation must be in sync.

To create a document, we must understand the purpose of document. Document without a purpose is a waste and must be avoided. For every document you can find someone, who will think it is valuable. The challenge is that value is invariably determined by the customer, not the producer. Dr. Dobb proposed formula to calcluate the [Cruft](http://en.wikipedia.org/wiki/Cruft){:target="_blank"} of a docuemnt.

```
CRUFT = * C * R * U * F * T

C = The percentage of the document that is currently "correct".
R = The chance that the document will be read by the intended audience.
U = The percentage of the document that is actually understood by the intended audience.
F = The chance that the material contained in document will be followed.
T = The chance that the document will be trusted.
```

The CRUFT rating of the document, with 100% being a bad thing, is calculated with the following formula:
```
100% - C * R * U * F * T
```

## How to build effective documentation?
> Create Views, Not Copies of documents. Reduce CRUFT in dcoumentation. 

We want to be as light as possible with documentation like other areas to minimize waste. Instead of describing same thing in three places, create the documentation in one place and reference it from others. This not only reduces tracability and maintainence burden but also increases consistency. Code is the best place to document, followed by Wiki based markdown, as opposed to traditional word and PDF documents. The documentation must also be barely good enough, but that does not imply low quality. The documenation is continiously changing in Agile/Lean and we should strive to keep it in sync with the system implementation. This is easy to say, hard to do; but builds with practice. Some tips for writing effective documentation:
- Write stable, not speculative, concepts. 
- Prefer executable work products over static ones.
- Document only what people need.
- Dont' try to get first time right, iteration is good.
- Document late. 
- Be concise!

## What is an ADR?
An architecture decision record (ADR) is a document that captures an important architecture decision made along with its context and consequences. ADR Captures the decision at the time it is made. A good, ideally describes following:
- Issue
- Decision
- Ontology - Category/Tagging etc.
- Assumptions
- Constraints
- Alternatives
- Reasoning/Argument for choice
- Implications


## How does refactoring affect architecture?
> The structure of components, their interrelationships, and the principles and guidelines governing their design and evolution over time.

Refacotring is the process of improving intrenal structure of code without altering the functionality. The refactoring is usually small changes, but sometime there are large scale changes. We decorate the term with the word “architectural” to make it obvious that we are describing larger-scale, system change. Refactoring is a key aspect of Agile Software delivery. We must be open to changing needs and facilitate refactoring, but at the same time define clear *Guardrails* to protect system from going into dangerous territory.

## References
- [Lean Architecture Article in dzone by Chris Shayan](https://dzone.com/articles/lean-architecture-1){:target="_blank"}
- [Agile Architecture in SAFe Agile Framework](https://www.scaledagileframework.com/agile-architecture/){:target="_blank"}
- [Role of EA in Lean Enterprise](https://martinfowler.com/articles/ea-in-lean-enterprise.html){:target="_blank"}
- [Agile Modelling](http://agilemodeling.com/essays/agileArchitecture.htm){:target="_blank"}
- [Agile Architecture](https://en.wikipedia.org/wiki/Agile_architecture){:target="_blank"}
- [Calculating Documentation Cruft](https://www.drdobbs.com/architecture-and-design/dr-dobbs-agile-modeling-newsletter/201001273){:target="_blank"}
- [Architecture decision record (ADR)](https://github.com/joelparkerhenderson/architecture-decision-record){:target="_blank"}
- [Github ADR Organization](https://adr.github.io/){:target="_blank"}
- [Open Agile Architecture](https://pubs.opengroup.org/architecture/o-aa-standard/index.html){:target="_blank"}
	













