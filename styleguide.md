---
layout: page
title: Style Guide
---

# The Hivemind Manifesto

## _Opinions are good. We have strong ones._
Over the last year, we've finished a few projects, started a lot more, and generally refined our ideas between each 
other about what we thought Hivemind is, what we think works, what we think sucks, and what perfection is to us. 

To us, opinionated design is the kind that sticks out, and we want to share those opinions of ours, because that's the point of the internet, right? 

## What exactly _is_ this? 
This is a style guide for your code, for your design, and for your life as a developer. It's a living, breathing, evergreen guide based on our opinions, experience, and how we do things here at Hivemind. 

# Navigation / Table of contents 

This document will be a living and breathing guide to the style and philosophy that our shop runs on. That being said, things will change, be updated, added to, and sometimes removed. 

1. [Philosophy and Personality](#philosophy) 
2. [Concept and Project Scope](#concept) 
3. [Design](#design)
4. [Development](#development)
5. [Deliverables](#deliverables)
6. [Deployment](#deployment)

## <a name="philosophy"></a>

# 1. Philosophy and Personality 

## Perfection and Minimalism  

To us, and to me personally, minimal approaches work out best, and we try to extend this philosophy into everything
we build here. 

From visual aesthetics, to the actual code powering our apps, we strive for the minimal but most effective approach.

This means a few things to us in application. 

* **1.1 Don't use generators**
They start you off with code bloat, and you'll spend just as much time deleting unused code as it would have taken you to just bootstrap the application structure yourself.
	
* **1.2 Simple and concise variable names**
	
* **1.3 Structure is just as important as contents**
Every app we produce follows a very specific folder architecture for both server and client code. 

* **1.4 You are not your online profile**
Your Facebook profile isn't going to outlive you. Your creation and progress as a human is what will outlast you. If you want to live forever, build a legacy first. 

* **1.6 Your time is too valuable to be dependent on digital notifications**
Your time is your most valuable asset. Your notifications should be kept to a minimum. Focus on your work and creating something that's meaningful. 

* **1.7 A screen will not be the first and last thing you see**
Technology is a way to enhance the experiences you have as a human on this planet, not hinder them. 

* **1.8 Tangible will always take precedence over digitaI'm al**
In application, this means your clients will always prefer getting a physical report to receiving just an email. They'll prefer seeing their business cards 
in person rather than seeing _mockups_ of their business cards. 

* **1.9 People don't know what they want until they see it**

* **1.10 Your time is the most valuable asset you have**

* **1.11 Be careful what you get good at**

* **1.12 You can either be good, get good, or give up. Choose one.**

* **1.13 Find inspiration everywhere**
Surround yourself with different textures, media, experiences, everything. Don't let it all be digital, either.

* **1.14 You are not your computer**
Your computer isn't you. You are a user, using the tools you have to create your vision and concepts.


## <a name="concept"></a>

## 2. Concept and Scope

* **2.1 The quickest way to decrease quality is to increase scope.** 

Your project's scope should be laser sharp and perfectly defined down to every user interaction. 
The concept should be solid, and you should have numbers rather than opinions to back up _why_ you're creating something. 

* **2.2 Numbers rule, people lie.**

Test everything, and use numbers to dictate decisions wherever feasible. 

- A/B test screens. 
- Run speed tests on code snippets.
- Factor in internet speeds and try using your app with dial up. See how fluid it is. 
- Keep metrics on important parts of your company. Don't worry about vanity metrics. 

* **2.1 Have a very well defined user flow and wireframe for every feature**

Don't even think about development until you have every feature and part of the app designed and wireframed. 
You should have a mockup with interaction details and a user story / epic for each feature before you even touch the command line or your text editor. 

	DO: Use Sketch to mockup sites and export CSS faster
	DO: Use InVision or a similar product to create interactive mockups if you feel they're necessary. 

	DON'T: Rely on "in your head" visions of what the app will look like. 

* **2.2 An increase in scope should mean an increase (push back) in deadline and an increase in cost.**

If the client is _insistent_ on adding a feature, inform them that it will cost both extra money and extra time. Do not ever allow the client to add a feature without charging them for it. If you do, you'll set the precedent of free and never be able to break it. This is why your initial scope and feature document should be as detailed and thorough as possible. 

* 2.2.1 Question every single possible situation you can think of when creating the original feature list. This will save you time and money down the line, even though it feels paintstaking at first. 

* **2.3 Every concept should be thoroughly vetted and examined by all persons.**

* **2.4 Use a web app template and expand on it.**

Check out our [web app tracker.](https://www.evernote.com/l/AOpJV1doW0dAjoKex5YN1qLLL6H0LGOdO6M)

- [ ] Figure out project scope and features 

	DO: Use a project tracker. 
	DO: Use a burndown chart or tracker. [Asana](www.asana.com) has this feature for projects for free.

	DON'T: Scribble down a list of features. 

	--------------------------------------------------------

	DO: Set specific use cases and stories for each feature. 

	DON'T: Fall victim to scope creep or allow the client to request additional features. 

	--------------------------------------------------------

	DO: Have a mockup for each feature. 

	DON'T: On-the-fly develop a feature. 

	--------------------------------------------------------
	
- [ ] Determine deadlines for project

	DO: `Project Deployed and Launched by Nov. 30th`

	DON'T: `Due in November`

	--------------------------------------------------------

	DO: Have milestones and goals for each step and feature of the project. 

	DON'T: Set a single deadline and gauge progress based on that. 

Your projects require constant feedback and tuning. You'll need to adjust milestones and deadlines based on progress, and with only a single deadline you really won't have any metric of your overall progress towards a goal. 

How do you eat an elephant? One bite at a time.

- [ ] Send contract to client with deadline for signing

	DO: Have a well designed, branded, lawyer-reviewed formal contract, with the milestones and goals you've set included in the project included in an appendix and stated in the contract. 

	DON'T: Send them a boring, simple contract that states the bare minimum details. 



## <a name="design"></a>

## 3. Design

3.1 Settle on a grid system and stick to it, whichever you choose.

3.2 Create a base set of templates and deliverables for each project type.

3.3 Create a style tile template that each project will start with. 

This should include a typography example, color palette, and examples of main UI elements such as buttons, links, checkboxes, cards, etc... 

Create a set of templates for your design projects that all projects should start on. I suggest creating templates for logo design, business card design, multiple sizes of collateral, letterhead, and icons for all devices and sizes.

Use semantic filenames.



## <a name="development"></a>

## 4. Development 

* **4.1 Deploy early, adjust frequently**

Get your entire planned stack for the app deployed and working together nicely. This will help you with rollout and you won't be scrambling last minute to get your app deployed on a deadline and will let you focus on features for the tight deadlines rather than dev-ops.

This also allows you the opportunity to showcase new features and progress to the client, as well as serve as a means of instant gratification and motivation for your team. 

* **4.2 Use Docker**

This ties in with the above point. Docker is killer for deployment, and will help you create a scalable architecture for your apps. Use it. 

* **4.3 Get in the habit of using a CI/CD system**

CircleCI or Travis are great starting points for a CI/CD pipeline. Pipelining will be huge for growing your projects and working on multiple projects at once. 

* **4.4 Write documentation before, during, and after your project**

Write a lot more documentation. a new person should be able to come in to your project, clone the code base, fix an issue on the server, or fix a bug in your code just because of how good your documentation is.

* **4.5 Create an in house style guide**

You should have a defined set of styles in house for variable and function naming conventions, semi-colon use, tabs vs spaces, etc... A good reference would be the NASA Code Style Guide. They have very defined conventions for every aspect of their code.

The style guide you're reading is our in house guide. We maintain and improve this on a regular basis. 

* **4.6 Run a linter**

If you're using an IDE, it already will have this, but for Sublime or Atom, you'll have to add this in. You can also add it to your build process if you're using Gulp or Grunt. Whatever way you choose, you should have one. 

* **4.7 Use a build system, and constantly improve it**

Tying in to the previous point, you most definitely _should_ be using a build system. It'll speed up your development time and make your troubleshooting process. Waste less time on the obvious bugs and spend your time debugging bigger issues. 

* **4.8 Maintain a blog**

* **4.9 Use semantic function names**

This ties in to your company style guide. You should have a defined convention (camel case vs snake case vs whatever) for styling and naming your functions and variables. 

Ideally, you should also have a style guide for different types of files. For example, if you're using Angular, you should have a defined style guide for services, controllers, directives, and templates. 

* **4.10 Comments should explain why, not what**

Comment your code, but comment why you made a change or why you do something. Your code should be readable enough to begin with that you shouldn't need to comment unless you have to do something different or something that the code doesn't inherently explain.

* **4.11 Develop to a design**
Don't design around the development. You should have very clear wire frames for every screen and function of your app.

* **4.12 Settle on a style guide. 


## <a name="deliverables"></a>

## 5. Deliverables 

* **5.1 Deliver a clear, concise final summary document. **

* **5.2 Send client the download link to the DropBox folder with all relevant code, materials, assets, and **documents. 

* **5.3 Have the deliverables ready 3 days before the due date.**

* **5.4 Use a concise and easy to understand folder structure for the deliverables. **

* **5.5 Include a follow up contract for maintenance and feature requests as part of the deliverables. **

## <a name="deployment"></a>

## 6. Deployment 

* **6.1 Use CloudFlare** 

* **6.2 Setup SSL / TSL Certificates** 

* **6.3 Use HTTP/2 if possible** 

* **6.4 Setup multiple users on your server instead of using `root` for everything** 

* **6.5 Cache your site for faster load times** 

* **6.6 Setup service workers to load data in the background and do other performance-oriented tasks**

* **6.7 Serve static files with NGINX** 