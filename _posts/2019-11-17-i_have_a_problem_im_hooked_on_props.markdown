---
layout: post
title:      "I have a problem, I'm Hooked on props "
date:       2019-11-17 16:49:03 +0000
permalink:  i_have_a_problem_im_hooked_on_props
---


My final blogpost, for now. What a journey it has been. Right before the project mode weeks started, I was doing my usual panic because I could not think of a good idea for an app, when finally it hit me. I would make a application for reviewing concerts because it wasn't something that I had not seen exist, and I was desperate to integrate an external API - to make it real, ya know? I quickly learned that it was not as easy as one might think. I ended applying for around 4 or 5 keys from different APIs. Apparently some services need to approve you and have all these stipulations for using their data. Jeez. It was about a week into the project and I still had not heard back from the one I wanted to use the most. Luckily, one of them answered and I was able to continue creating my magnum opus. So first bit of advice, if you're going to use an external API, apply for a key ASAP.

Another bit of knowledge: React Hooks. They're cool. A little funky using them with redux, but I figured it out and I will share that knowledge with you now:

A problem I ran into was that my state was not being persisted when I refreshed certain pages. So let's say I was on a show page for a concert, as soon I refreshed, that concert was gone! No bueno. So, at this point I was tempted to turn my stateless, functional component into a class component and invoke componentDidMount. However, one proviso of this project asks for at least 5 stateless routes. If I pursued this pattern, I would not hit the requirment. After, asking for some advice from my cohort lead, I decided I would add the semi-new React Hook. From the React Docs:

"Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class." Perfect.

Now since this project requires you to use Redux as your state manager, I knew I would have to somehow incoporate both of these sweet features from these frameworks into my components. To start I want to highlight the `useEffect` function from React Hooks. The docs:

"By using this Hook, you tell React that your component needs to do something after render. React will remember the function you passed (we’ll refer to it as our “effect”), and call it later after performing the DOM updates. In this effect, we set the document title, but we could also perform data fetching or call some other imperative API." 

Basically, my functional component will invoke this function based on when certain conditions change. Now, I needed to figure what my trigger would be. I realized the best indicator of what my user should be seeing was- you guessed it- the URL. Cue React-router props.  Using react router to maintain the appearance restful routing also gives you access to `ownProps`. These props contain information about the url including `location` i.e. the path you've set for the route and even params- like an id! So mapping those props onto a variable which I placed in my `useEffect` function let me trigger a fetch everytime the route params changed! 

Another aspect of React Hooks is the `useState` function, but seeing as we were using redux to manage state, I *barely* used it. I encourage anyone reading this to look it up! It is very useful!






