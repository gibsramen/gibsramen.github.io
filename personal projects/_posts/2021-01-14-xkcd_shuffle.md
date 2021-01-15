---
layout: post
title: Learning React.js and xkcd-shuffle
date: 2021-01-14
categories:
  - Personal Projects
tags:
  - code
  - javascript
  - typescript
  - react
  - aws
---

![xkcd_shuffle_page](../assets/imgs/xkcd_shuffle.png)

Over the holidays I decided to learn React.js.
I didn't have any particular reason or use case in mind, I just thought it would be neat.
JavaScript & "front-end" technologies had always been a black box, even as I started to read and use more JavaScript for EMPress.
There was no particular reason for choosing React over something like Angular or Vue, I had just heard React so decided it was as good a place as any to start.

My first couple days were pretty standard: install things and copy tutorials.
I've been using [create-react-app](https://reactjs.org/docs/create-a-new-react-app.html) to get started as it allows me to focus on the front-end code independent of some of the other stuff that goes into web development (packaging, minification, etc.).
The first minor project I worked on was a small app to search for an album and display the popularity scores from Spotify's API.
This was especially fun because I hadn't done much work with APIs much before.

While writing this app, I had an idea for a new project - a webpage that could return several random panels from existing XKCD comics and fuse them into a new chimera comic.
To generate these panels, I used a tool called [Kumiko](https://github.com/njean42/kumiko) which splits comics into their constituent panels.
This wasn't a perfect solution because a lot of XKCD comics are laid out in a non-standard (linearly sequential rectangular panels) but it was a good start.
I downloaded all the comics from the XKCD API and used Kumiko to cut and save the panels.
I manually went through all the ~2400 comics and took note of the ones that were poorly cut and excluded those (for now).

From here, I had to figure out how to host and retrieve the panels.
I had used AWS briefly in the past so I figured it wouldn't be a bad idea to go a little more in-depth.
I uploaded all the panels I cut to an S3 bucket as well as a file listing which ones were present.
At this point I figured I had a couple options - I could either write the front-end to pick 3 random images or do that on the back-end.
I opted for the latter as I felt it would be cleaner.
When the user presses the submit button, a POST request is sent to an AWS API which activates an AWS Lambda function that returns 3-5 random panels from the bucket (depending on the user input) through AWS CloudFront.
These images are then displayed on the web app with a note of each respective comic from which the panels were taken.

With the main code done, one of the things I noticed was that the images weren't loading synchronously.
This caused a pretty unpleasant experience where pressing the submit button would show the process of replacing everything.
To address this I added a simple loading indicator so that the full comic only appears after all the images are loaded (through the `onLoad` property of `<img>`).
This looks a lot nicer and I may eventually add a spinning indicator or something like that instead of a plain "Loading..." text message.

I felt that with the loading indicator done (that took me a lot longer than I would have thought to get working...) the project was at a good-enough point for this blog post.
That being said, there are still a couple of things I'd like to add.
For one, a way for the user to notify me that a panel looks funky.
Because I automated the process and went through the panels pretty quickly, it's possible that part of one got cutoff and looks weird.
Currently I have my e-mail listed at the bottom for people to contact me in this case, but it might be cool to have a "report" button to streamline the process.
Additionally, I would like to indicate visually which comics are included in the possible pool.
That way, if people have a specific comic they want to be added (for me to manually cut) I can get to that sooner.
Finally, I need to add some sort of error handling (maybe testing as well?) to make sure things fail gracefully.
(Oh I also need to make it look not terrible on mobile...)

Overall this was a really fun project and I hope to keep learning about React and front-end development.
See the project [here](https://gibsramen.github.io/xkcd-shuffle/)!

## Technologies used:

### Front-End

* React.js
* create-react-app
* JavaScript
* TypeScript
* Axios

### Back-End

* AWS S3
* AWS Lambda (serverless)
* AWS API Gateway
* AWS CloudFront
