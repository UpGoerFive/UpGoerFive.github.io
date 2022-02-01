---
layout: post
title:  "Lessons Learned Writing a SciKit-Learn Model Manager"
date:   2022-2-2 9:52:18 -0600
categories: [Flatiron, sklearn, OOP]
published: False
---
Phase 3 of the Data Science Live Program at Flatiron just finished for me. The project for this phase involved creating classification models using sklearn, and I decided to try to write my own framework to manage our models. On one hand, I learned a lot, on the other, I ended up causing some problems for us along the way. There's a good amount to learn from in this project, so I'll be taking a look at the code for the modeling class we used. The code can be found (here)[https://github.com/UpGoerFive/Tanzania-Well-Project/blob/main/ModelClass.py]. Obviously on a team project, not all of this code is mine, but I'll be focusing on my own contributions and the problems they caused for this post.

### Why do this?
On the Phase 2 project, my notebooks ended up being pretty messy as I was figuring out a lot of stuff as it went on. Part of this is related to my own use patterns for Jupyter notebooks. These are of course nice to use for exploratory tasks, and as someone whose first programming language was scheme, I do love something approaching a REPL. My notebooks almost had the feel of being records of a REPL session, and rather than go back and refactor, I normally just kept adding more on. For this project, I decided to not let myself create this much of a mess. My thought was to actually use a normal .py file to write code that I could share with my teammates to help get things done more efficiently. Instead of just relying on my saved history approach, I'd actually try to plan things out. The added impetus for this was a lack of confidence in my sklearn awareness. I could write the correct way to do it once, and then just call the method again rather than having to execute the pattern correctly a second time. On the whole, this was actually a good impulse; why use a programming language and not automate the easy bits? Predictably, I predicted badly.

### Setting it up

### Lessons

#### Planning for flexibility in your structure != flexibility in using your structure

#### Don't repeat yourself means don't repeat your functionality

#### Minimize new stakes if possible

### Improvements

So now my code is in a workable state, but clearly things are far from ideal. Since I'd like to use this code in the future, I need to make sure that it's usable for modeling more than just a specific water well dataset. There are a few options for how to proceed. Some are likely to be good ideas no matter what, others I'm not sure yet, I likely won't know until I try them.

##### Structure Change Options:
- Split the manager into a training manager and an evaluation manager. This could add greater modularity to the code and allow older models to still be assessed by whatever new metric I need.
- Manipulate the models with classes (dataclasses might be an even better option)
- Both ideas so far involve adding onto what is arguably an already over engineered way of getting things done. Ditching the object oriented style and just using what I have so far as a template could be effective. This is probably my least favorite option.
- Try doing the same thing in a functional style. This is not what I'd normally associate with Python, but it's possible. I'd want to have a clearer idea of how this would solve my problem of needing to rerun the entire thing at each revision before I pursue this.

##### Quality Checking:
- Write a test suite. I've used pytest sparing before, and it wouldn't be too much to set up tests for this project. This is really something I should have already done. I didn't, in part because as discussed before I was really learning sklearn through writing this, and because I was worried about rerunning models to run the tests. I ended up creating small development notebooks when adding features that essentially were just manually run tests, so there's no reason not to do this.
- Make sure only one method has control over a specific functionality. This could have saved me a headache or two in usage, as we saw above. This approach does not completely line up with the sklearn interface as it is, and also code be more of a spaghettification than anything else. Taking our example from above, if I wanted to make sure my train_model method was the only thing setting a trained model or my train score, I'd end up sending the best estimator from the search, and it's score to the train_model method to do the assignment part, while not retraining the model for obvious reasons. This overloads the method. It's most common usage will train a model, but sometimes it will be asked to simply *say* it's trained a model and do work that could easily have been done by the method that came up with the answers.
- Get pickle working. Worth considering for the same reasons I originally wanted to do it.
- Do more comprehensive logging. This is sort of low hanging fruit, since I already was trying to do this, it just wasn't done to an extent that saved us problems. If everything we want from the model gets logged, we can still go back and grab information from the logs even if we throw the model object away by accident. This could be extended to outputting our predictions to a .csv file for later use if we wanted. We wouldn't be able to make new predictions, but at least we could still run new evaluation metrics without having the model.


<script src="https://utteranc.es/client.js"
        repo="UpGoerFive/UpGoerFive.github.io"
        issue-term="pathname"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>
