---
layout: post
title:  "Lessons Learned Writing a SciKit-Learn Model Manager"
date:   2022-2-2 9:52:18 -0600
categories: [Flatiron, sklearn, OOP]
published: False
---
Since November, I've been in the Data Science Live Program at Flatiron School. The Phase 3 project just finished, which involved creating classification models using `sklearn`. I decided to try to write my own framework to manage our models. On one hand, I learned a lot, on the other, I ended up causing some problems for us along the way. There's a good amount to learn from in this project, so I'll be taking a look at the code for the modeling class we used. The code can be found [here](https://github.com/UpGoerFive/Tanzania-Well-Project/blob/main/ModelClass.py). Obviously on a team project, not all of this code is mine, but I'll be focusing on my own contributions, and the problems they caused for this post.

## Why do this?

On the Phase 2 project, my notebooks ended up being pretty messy as I was figuring out a lot of stuff as it went on. Part of this is related to my own use patterns for Jupyter notebooks. These are of course nice to use for exploratory tasks, and as someone whose first programming language was scheme, I do love something approaching a REPL. My notebooks almost had the feel of being records of a REPL session, and rather than go back and refactor, I normally just kept adding more on. For this project, I decided to not let myself create this much of a mess. My thought was to actually use a normal .py file to write code that I could share with my teammates to help get things done more efficiently. Instead of just relying on my saved history approach, I'd actually try to plan things out. The added impetus for this was a lack of confidence in my `sklearn` awareness. I could write the correct way to do it once, and then just call the method again rather than having to execute the pattern correctly a second time. On the whole, this was actually a good impulse; why use a programming language and not automate the easy bits? Surely this will cut down on our modeling time and let us compare results more quickly? Predictably, I predicted badly.

## Setting it up

I started writing the class without a very concrete idea of where to go, but I can summarize my approach this way:

- Write something to allow my teammates and I to add and run a model in as few lines of code as possible.
- Preserve model scoring information somehow.
- Make something generalizable enough that I could use it for other projects in the future.

I didn't have to start from nothing thankfully. ScitKit-Learn itself already has a pretty consistent interface, and I had some example code from Flatiron to look to for ideas. The basic idea was just that, basic. I wanted to essentially automate and consolidate our modeling process. No more 200 cell notebooks with duplicate code!

## Let's take a look

Rather predictably, the class includes an `add_model`, `train_model`, and `test_model` method, as well as methods to do these things for all managed models. There's a `hyper_search` method to do hyper parameter tuning (default is `RandomizedSearchCV` but any search class can be passed in.) My teammates contributed the `model_evaluation` and `permutation_importance` code, which I integrated into the class, and made a few small changes to later (foreshadowing).

That's the basic gist, how did I do as far as the goals I mentioned up above?

### Did writing this manage to streamline and organize our modeling code?

I believe so. The basic version looks like:

{% highlight python %}
X = pd.read_csv('data/Training-set-values.csv')
y = pd.read_csv('data/Training-set-labels.csv')

X['date_recorded'] = pd.to_datetime(X['date_recorded']).astype(np.int64)

modeler = ModelClass.Modeler({'Random Forest': {'classifier': RandomForestClassifier(max_depth=4), 'preprocessor': None}}, X=X, y=y)
modeler.model_evaluation('Random Forest')
{% endhighlight %}

Here `X` and `y` are imported separately due to how our data was provided to us. The date conversion could have been added to a preprocessor, but this is the pattern we were able to use for our notebooks. Additionally my current version of the code doesn't require a `'preprocessor'` key to in the model dictionary, but at the time this was not the case so I'm showing that version. In general, we would be handing in a dictionary full of different models, so not quite as concise as here, but this still works. Here the `modeler` object:

- performs a train test split that will be saved and consistently used for all models
- provides the `'Random Forest'` model with a preprocessor since one wasn't given
- fits the model
- tests the model
- prints out a classification report and plots a confusion matrix

Granted none of this code in `sklearn` is particularly difficult or ridiculously long, but we didn't have to write any of it, and another model can be added and similarly evaluated in two lines. Our modeling notebooks for this project were both around 30 cells, with maybe one or two long ones that had the model dictionary and hyper parameter search options written out, and that's *including* markdown cells.

### Were we able to save our model information?

I think I had a decent idea with this one, and I got to play around with logging in python. [This](https://realpython.com/python-logging/) Real Python article and the official documentation gave me pretty much everything I needed. The only problem being that since I was trying to log from a Jupyter notebook, I had to do a ~~frustrating hour of debugging~~ little bit of work to figure out that I needed this: `logger.setLevel(logging.INFO)` in order to wrest logging control back from my notebook. With logging now working, I could start recording ... not quite enough. As with a lot of things on this project, I didn't realize this at first. Later on I attempted to get pickle working to fill the gap, which is probably what I should have done instead of logging in the first place (more foreshadowing). Using all the logging statements did have the nice effect of letting me add optional printing to pretty much every method. All I had to do was add and remove the stream handler.

### Is the code usable for the future?

My lame answer to this is that we don't know yet because I haven't done another project and we won't know how well it performs until then and it'll be better by then and next time will be different and I'll have fixed everything and on and on. The real answer is sort of. As I showed above, we were able to cut down on the amount of extra and repeated cells, when the class worked (foreshadowing :heavy_check_mark:). And in fairness to my lame answer, I really can't tell if I'll need to do an amount of tinkering in the future that means this extra layer just gets in the way.

## Lessons

Alright I've been ***foreshadowing*** about something for a while, what is it? The morning of the presentation we woke up to discover that I'd broken the metrics methods the night before. To see how, it's important to know a few things:

- When I started writing this class, I was a little fuzzy on how things should be setup in SciKit-Learn. As I said, this was part of why I started writing this thing early on in the project: I kept trying to set up basic things and realizing I was going to have to write minor variations of the same thing 10 times if I didn't get organized now. Consequentially, the first versions I wrote would take the preprocessing `ColumnTransformer` from the model dictionary, transform the X data, and then hand that off to the classifier. This is pretty much exactly what pipelines in `sklearn` are for.
- Towards the end, I was starting to see the issues with logging that I've discussed above. I thought getting `pickle` working would be better than trying to throw every little detail into the log files.
- One of the things I thought was nicest about the class was the `create_default_prep` method. This unsurprisingly creates a preprocessor for a model if none is given when the model is added.
- I had originally been using [`check_is_fitted`](https://scikit-learn.org/stable/modules/generated/sklearn.utils.validation.check_is_fitted.html) to make sure that `model_evaluation` was looking at a trained model.

Spot where I went wrong?

While we're exploring that, I'll do my best to be positive and look at what I was able to learn by going through this.

### Think about what you want out of your code early

My late decision to make our models pickleable kicked off this whole series of problems, or more accurately, exposed them. In general, I didn't consider the information I needed to get back out of the class enough. The initial version didn't have `remove_model` or `get_model`, only printing with `show_model`. Training and testing only gave back the cross validate scores and test accuracy. This was fine so long as we only cared about the model accuracy and could rerun our models easily. At the point I wanted to add pickling in, I had already baked in a pretty big roadblock for myself:

<img src="/assets/images/pickle-error.png" alt="You can fill in the 'Technical Debt' square on your bingo card" width="50%"/>

`pickle` can serialize SciKit-Learn object, so this one took me a while to figure out, but because the models had been given a default preprocessor with the `create_default_prep` method, that preprocessor is a local object, and we're locked. I'm not entirely sure how to solve this one other than by getting rid of the method, but at the point I was coming across this, I couldn't afford to spend more time on this issue, so there may be another solution I've overlooked.

### Handle one problem at a time

So that's a bit of a downer, sure, but how did it end up causing problems with the models next morning? That's right: I had decided to refactor while I went through the `pickle` struggle. By this point I knew more about how a pipeline in `sklearn` works, and changed everything over to the "correct" way of doing things. In my defense, I had initially started doing this because I thought something about how I had stored different bits of the model in the old way was causing my `pickle` issues.
The model still could be fit, still could have cross validate run, and still could be tested, but the next morning we found out that `model_evaluation` was very unhappy. At an unfortunate halfway where I hadn't fully committed to refactoring yet, I was manually creating a pipeline inside `model_evaluation` and `check_is_fitted` started throwing errors, even on pipelines I knew had a fit classifier. Some quick googling told me that `check_is_fitted` is normally an "under the hood" sort of thing, and it was better to just let `sklearn` check if a model was fitted by itself. Fair enough, but I wanted to fit a model if it hadn't been when `model_evaluation` saw it, and at this point in the project timeline I had completely forgotten that `try`, `except` blocks existed. Well simple enough, I'll just check for the `'train_output'` key in the model dictionary and we can send it to `train_model` or get its score back that way.

In the process of streamlining my train methods, I forgot to add back in the line that created a `'train_output'` key in the model dictionary.

### Minimize the stakes for new changes, if possible

Most of this wouldn't be an issue if it had been limited to my machine, but I happened to finish my ~~pickle~~ refactoring session before we were going to all run models over night. Normally when I added new code to the Modeler class, I'd have a notebook to test out what I was writing. I was still doing this, but wouldn't you know it, I missed ever testing a model with `model_evaluation`. As a result we ran the models on the new version of the Modeler code, and next morning couldn't get `model_evaluation` to run (or `permutation_importance` for similar reasons). It wasn't the end of the world, since we could get the models out, but then we had a long period of very carefully copy pasting the evaluation code and renaming what we had to. That morning ended up being damage control, rather than finishing up and checking the rest of the project.

Even then, this was still not an ideal setup even if everything had gone well. Ultimately to get new information out of our models, we had to rerun our notebooks to get the new modeler code. This more or less meant we had to reset our actual model instances each day, even if we had the same hyperparamters. Without pickling, the models themselves basically didn't have version control, even if our code did, just based purely on how long it took to run some of them. An hour or two we could deal with, but any longer than that and we're more or less limited to what we had on hand at the moment. This highlights to me that the class is too big. Splitting it up into smaller parts would likely have let us deal with problems more effectively.

### Improvements

I did a bit more work on the class after the project ended, so now it's in a workable state, but clearly things are far from ideal. Since I'd like to use this code in the future, I need to make sure that it's usable for modeling more than just a specific water well dataset. There are a few options for how to proceed. Some are likely to be good ideas no matter what, others I'm not sure yet, I likely won't know until I try them.

#### Structure Change Options

- Split the manager into a training manager and an evaluation manager. This could add greater modularity to the code and allow older models to still be assessed by whatever new metric I need.
- Use `dataclasses` for the models. No more worrying about keys that don't exist.
- Both ideas so far involve adding onto what is arguably an already over engineered way of getting things done. Ditching the object oriented style and just using what I have so far as a template could be effective. This is probably my least favorite option.
- Try doing this whole thing in a functional style. This is not what I'd normally associate with Python, but it's possible. I'd want to have a clearer idea of how this would solve my problem of needing to rerun the entire thing at each revision before I pursue this.

#### Quality Checking

- Write a test suite. I've used pytest before, and it wouldn't be too much to set up tests for this project. This is really something I should have already done. I didn't, in part because as discussed before I was really learning `sklearn` through writing this, and because I was worried about rerunning models to run the tests. I ended up creating small development notebooks when adding features that essentially were just manually run tests, so there's no reason not to do this.
- Make sure only one method has control over a specific functionality. This could have saved me a headache or two in usage, as we saw above. This approach does not completely line up with the `sklearn` interface as it is, and also code be more of a spaghettification than anything else. Taking our example from above, if I wanted to make sure my train_model method was the only thing setting a trained model or my train score, I'd end up sending the best estimator from the search, and it's score to the train_model method to do the assignment part, while not retraining the model for obvious reasons. This overloads the method. It's most common usage will train a model, but sometimes it will be asked to simply *say* it's trained a model and do work that could easily have been done by the method that came up with the answers.
- Get pickle working. Worth considering for the same reasons I originally wanted to do it.
- Do more comprehensive logging. This is sort of low hanging fruit, since I already was trying to do this, it just wasn't done to an extent that saved us problems. If everything we want from the model gets logged, we can still go back and grab information from the logs even if we throw the model object away by accident. This could be extended to outputting our predictions to a .csv file for later use if we wanted. We wouldn't be able to make new predictions, but at least we could still run new evaluation metrics without having the model.

### Moving Forward

I do think there was a lot of value in doing it this way, even if it provided a lot of knowledge on how not to do things. I'm sure that the current version of the code isn't perfect, so if you have a suggestion, please let me know! I'm always happy to learn more. I'll likely post updates about working on this class as I hopefully don't cause too many more problems. Thanks for reading!

<script src="https://utteranc.es/client.js"
        repo="UpGoerFive/UpGoerFive.github.io"
        issue-term="pathname"
        theme="github-dark"
        crossorigin="anonymous"
        async>
</script>
