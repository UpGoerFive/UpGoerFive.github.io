---
layout: post
title: "Data Science - Start"
date:   2021-11-28 22:52:18 -0600
categories: jekyll update
---


## Hello!

I'm Nathaniel, and I've just started the Data Science program at Flatiron School. I wanted to be a violist for a long time and even made it through a music degree before realizing that wasn't the path for me. I've been interested in programming for a while, but hadn't been able to put any time into it until I finished school. I graduated early in the pandemic, and a change into programming felt appropriate. I worked through a scheme book, and took a few python courses. 
I really wanted to attend Flatiron as a way to push myself, and as an opportunity to learn data science. I'd been briefly exposed to a few topics beforehand, but most of what I'd been doing had been much closer to development, though still very rudimentary. Learning data science as a whole and getting job training at the same time seemed to be an obvious choice. This blog was started as part of the Flatiron course, but I'll also post other topics, thoughts on data science, how the course is going, or something unrelated I'm interested in.
The goal of this post is to share why I'm interested in data science, and what I'd like to learn.
## Curiosity

I'd like to start by going through a few things I've seen that make me interested in data science. These are mostly visualizations, though I do of course want to do more with data science than just make pretty graphs :)

### F1

I've been a Formula 1 fan for a while now. F1 like many other sports likes to display on screen graphics to illustrate characteristics of the on track action. This includes comparisons of how drivers brake going through corners, estimated overtaking difficulty, and reaction time comparisons at the start. The ti(y)re wear graphic has probably gotten the most attention, however. 

<img src="/assets/images/racefansdotnet-f1-graphics.jpg" alt="Hamilton Pitting for New Tires" width="50%"/>


This was a graphic introduced in 2019 which was widely criticized by fans and even initially produced some confusion at [Pirelli](https://www.racefans.net/2019/10/26/f1s-new-tyre-wear-television-graphic-is-misleading-says-pirelli/), the tire manufacturer for the sport.
Part of the reason this graphic received so much criticism, is that tires are a very complicated part of the strategy to begin with, and the goal of a graphic like this is to simplify the information for viewers. This lead to early displays that were clearly wrong. Most notably in the first race these graphics were used, Lewis Hamilton pitted because his tires were worn, and the graphic on screen had green tires with 70% left on them. This situation is interesting to me more because of the problem rather than the attempted solution.
Tire wear has many different factors, some of which are being taken into account for these graphics, at least according to this [article](https://www.formula1.com/en/latest/article.explaining-the-new-tyre-performance-graphics-seen-on-tv.21CVJlHg0St8zrzaaVbU4L.html), however it's still not clear what data is being used and when. Pirelli typically updates their recommendations after Friday practice when they receive sets of tires back from all the teams. It's not clear if AWS uses this in their model, or really what data specifically is being used. A strict interpretation of the above article would mean this model essentially only uses what's available to fans through the live timing app.
As pointed out [here](https://www.youtube.com/watch?v=ppP0lMXlGtQ), the biggest obstacle to making this an effective tool to learning more about the race isn't necessarily how accurate the graphic, but rather that on the outside it's difficult to know how representative the whole thing is. A few flubs and even if the model has improved over the past few years (who knows?) it's still a widely derided part of the viewing experience. This lack of transparency is a quality of data science work I'm interested to know more about; it's also an area I'm weak in. I'm here to learn though! 

### Where to go?

By contrast, this New York Times [tool](https://www.nytimes.com/interactive/2021/11/23/opinion/sunday/best-places-live-usa-quiz.html) is much more open. The about page includes brief sections on methodology and what sources they used for their data. This tool is also on the opinion page for a reason. The bias in the tool's construction is acknowledged and understood in the context of the tool. Rather than designing something to draw objective conclusions from, the creators made something to satisfy curiosity and work with the subjective wants of the user to get results. Another good choice was including breakdowns of the overall match estimation. This lets users engage with the data more, and they can correct if the way they filled out the options weighted their results in an unwanted way.

The location picker tool succeeds where the F1 diagrams fail by letting the audience understand some of the decision making and presenting the data with clarity but without over simplification. There may be more issues at hand with either project, but the transparency difference I find most interesting right now. I might find implementation ideas more appealing as I move through the course, but for right now my best understanding of the effectiveness of a visualization is as a user.
