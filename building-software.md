# Planning a Software Project

Hello, and welcome to my guide for planning software projects! I'm Kevin, a full-stack developer who has worked as the sole developer in startup environments a few different times. I am approaching this guide from a solo-developer point of view, but these techniques also work with teams of people - you just have to be better at filtering the ideation pieces and knowing your team members' strengths if you'd like to get the most out of it.

Coming up with build plans and executing on them has always been a curiosity of mine, so I figured I'd share. There are a few key concepts to go over, namely ideation, scoping functionality, speccing and testing requirements, and writing business logic.

## Ideate

The first thing any software build needs is an idea - something to work toward. This can be a problem you wish to solve, or something simply to practice your skills. If you are having issues coming up with ideas, try using a random word generator or think of a problem in the real-world that might be helped with a bit of automation or organization.

Some simple ideas off of the top of my head are:

 - Tinder for Dogs - Cute idea, and who doesn't like dogs?
 - Location Ping App - Token-based app that allows someone with a special key to ask to see where you're located. 
 - A bot for Discord or Slack
 - Web Scrapers - Shopping sites to track trends, news sites for keywords, sports betting statistics, etc.
 - FishFinder - allows you to mark GPS locations where fish were caught along with a photo.
 - Virtual Pet - Create a raise a virtual pet that can do things like eat, train, and gain abilities. 

So, now that you've stolen my ideas, or come up with your own, you can start asking the real question: What do I expect this thing to do? Start making a list!

First, lay out ideas and ways that your app is ACTUALLY going to do at a high level. We are not looking at technical specs or thinking about technologies to use right now, we are only putting down core ideas that help us understand what we want to the app to do.

For example, if I decided to build Tinder for Dogs, I would start out with a simple mission statement that goes something like, 

"An app for dogs and their owners to match and meet." Simple enough, but it helps us lay out what the app is, and it gives it just a little bit of substance for us to base our decisions on later. 

After we have the identity of the app established, we can start writing User Stories. User stories are generally written in the third person and looks something like:

- Users should be able to sign up and create a profile.
- Users should be able to upload photos. 
- Users should be able to report people (or dogs) if they are abusive or badly trained.
- Users should be able to match with other users.
- Users should be able to message users who they have matched with.

If there was another type of user that wasn't just 'user', then we would change the noun in the above user stories like this:

- Dogs should be able to record a bark.
- Dogs should be able to vote.
- Dogs should not be able to create accounts or vote without their owner's confirmation. 

I imagined an app where you can put it in 'dog mode' where you, split the screen, give them a photo they can 'paw' to select, then the owner confirms it by tapping a small confirmation button on-screen (something the dog won't be able to hit itself, I guess). It's a bit of a reach, but apply it to things like admin accounts or multiple user types. You'll soon find that you have actions you'd like your users to take in multiple contexts.

Each of these user stories comes with their own nuance and implications, but we just need a basic list to start with. The ones above aren't a bad start. Don't be too specific! Just get the ideas down so we can synthesize them in the next step.

Now that we have the basic skeleton of expectation laid out, we can start looking at what it's actually going to take to get this app up and running.

For example, having a user upload a photo is actually fairly complicated:
- For User photo uploads to work:
	- Carrierwave must be set up correctly for HTTP
	- S3 bucket must be created
	- S3 bucket permissions must be set
	- UI for  photo uploads should be built (this should be a whole new item in and of itself)
	- Users must be able to delete photos
	- They must be able to set a 'default' photo

Write this all out and get a good idea of what it's going to take to get your features out of your head and into reality. Try to cover as many bases as you can in this step, because the moment you begin to think about estimating time you may find that some features will either take a very long time or need more research before you can execute well on them. 

Now that you have all of your thoughts and concerns down, it is time to start estimating how much work it'll be to actually build the thing out. 

## Scoping

Now it is time to extrapolate and list out what it is going to take to get each user story come to life! Start with the basics like user account creation, as the whole app relies on individual users being able to do some set of things. Something like this will suffice:
  - For users to sign up:
	  - Devise will need to be used for authentication and account creation. 
	  - Users will need to have attributes like location, name, description, and height.
- For users to match:
	- A Match model but be linked to a User
	- A Dog model must be linked to a User
	- A Preference model must be linked to a User
	- A Photo must be present for the User to view matches
	- A Photo much be present for a User to rate potential matches

As you can see, this starts to get complicated. Just by listing what our User needs, we have inadvertently created expectations that the other data will be linked to our User in some way. The goal of this phase is to eliminate confusion about how everything should tie together in the long run and to establish how we expect the data to flow. Get as granular as you can with it without laying out specifics about implementation, as we are trying to refine the idea as opposed to worrying about engineering solutions right now.

If you are having issues figuring out how it will all be connected together, try making a UML diagram (check out [draw.io](https://www.draw.io/)) and create some visualizations to help you out. 

What we are doing is identifying problem areas and potential complexity. We don't want to implement features that have low impact but take a lot of time to build (like the 'record a bark' feature) may not be suited for an initial build, but could be great ideas for later. Get it all down, as this is the time to do it because the moment you start writing code, it becomes much harder to change what's already been done as we have to create dependencies. If you can design for change by setting expectations in certain areas of the code, you can more efficiently implement these extra features later. 

Once you start to see how connections are laid out between nouns (models) and verbs (actions, logic), you can make descriptions of the intermediary actions and data expectations to lay out details about how it should be designed. Something like this may serve you well:
- Users -> Dogs, Users have many Dogs, but Dogs will only have one User through the table Owners.
- Users -> Preferences, Users have one set of preferences that will control user filtration, IP settings, location settings, and other display settings.
- Users -> Photos, Users have many Photos and Photos have only one User. 
- Users -> Match, Users should be linked many-to-many with other users through the table Matches. 

Now that you have a list of actions, nouns, and expectations about the app as a whole, start picking through and giving each line item a time estimate. This will help you understand and delegate your time more efficiently, as a task like setting up a User model with Devise (1-2 hours) is much less involved than figuring out audio capture and storing that sound data on Amazon S3 or something (12-15 hours if you need to do research), not to mention getting it to play back at the tap of a button (2-3 hours).

Do your best to itemize each piece, but don't feel bad if you have no idea how long something will take. A good starting point for junior developers is 8-12 hours per related feature set (how you grouped them naturally), 8 hours for a mid-level, and 4 for a senior proficient in the language. Time spent is VERY dependent on the task at hand and is by no means a static guideline - just a suggestion to base your expectations on. If you really are not sure, overestimate by a little bit as you will rarely find a time when you can simply build something and have it run perfectly - in fact it will probably almost never happen unless you are super familiar with the tools you are working with already. Sometimes sticky bugs come up, so adding an hour or two on to something you are unfamiliar with is perfectly acceptable.

Most tasks should be broken down into 3-4 hour blocks, but large or key features are going to take longer to get right. If you find you are having to guesstimate too much, try pulling back and breaking that item down into more parts. Most of the time, there are multiple complicated steps that need to be taken to even get a single feature working on a basic level.

> Written with [StackEdit](https://stackedit.io/).
