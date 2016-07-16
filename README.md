# 2016 San Francisco Data Sciece Summit Notes + Tutorials #

This repo contains notes and tutorials from the <a href="http://conf.turi.com/2016/us/" target="_blank">2016 Data Science Summit</a> 
in San Franciso, CA, which was organized by Turi (formerly 
known as Dato, creators of GraphLab Create). Below are my notes from the conference
in their entirety.

---

**July 12-13, 2016 @ The Fairmont Hotel**

***Note: Turi was formerly known as Dato, creators of SFrame and GraphLab Create***

# Table of Contents #
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Keynote -- Carlos Guestrin](#keynote----carlos-guestrin)
- [DeepDive -- A Dark Data System](#deepdive----a-dark-data-system)
- [Machine Learning for Analyzing Complex Time Series](#machine-learning-for-analyzing-complex-time-series)
- [Increasing Diversity in Data Science](#increasing-diversity-in-data-science)
- [Interesting Tools and Research](#interesting-tools-and-research)
- [Churn Prediction Tutorial](#churn-prediction-tutorial)
- [Machine Learning in Production](#machine-learning-in-production)
- [The Five Tribes of Machine Learning, and What you can Take from Each](#the-five-tribes-of-machine-learning-and-what-you-can-take-from-each)
- [Small Team, Large Impact - how we solved it](#small-team-large-impact---how-we-solved-it)
- [Industry Panel with representatives from Quora, Turi, Tableau, and Pinterist](#industry-panel-with-representatives-from-quora-turi-tableau-and-pinterist)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


---

# Keynote -- Carlos Guestrin #
**CEO Turi, Amazon Professor of Machine Learning, University of Washington**

* <a href="https://twitter.com/guestrin?lang=en" target="_blank">@guestrin</a>
* In 5 years, every application is going to be intelligent (will utilize some form of machine learning)

## Decision Trees Utilizing Node Decisions as non-linear Features for other models ##
* Transferable feature engineering

## What makes a ML platform agile? ##
* Turi now offers microservices (Turi predictive services) where you can utilize GraphLab Create
to deploy code to the cloud
* I'm guessing this would allow you to run ML models on Turi servers with GPUs? Because that'd be cool.

### Updating Models ###
* Models are stale the moment they are trained
* Prediction accuracy can improve up to 40% by leveraging real-time feedback
* Common approaches to fast adaptation
    * Frequent retraining, high-latency
    * Online learning, not general and difficult to debug

### Online re-ranking ###
* Issue: preferences change depending on session
* Adaptive ML in production
    1. Train and deploy your model
    2. Online reranker black box
    3. Consume
* Online reranker uses activity data
* Train using non-linear data (like from decisions made by a decision tree model)

## Distributed deep learning ##
* Distributed training throughput increases linearly as number of GPUs increases
* Distributed training convergence time decreases linearly as number of GPUs increases
* Utilizing the Turi RPC layer via Python, you can get a 40-60% improvement in TensorFlow performance
over the standard RPC layer

## How can you trust your model? ##
* Netflix is trying to make customers more comfortable with an AI solution by making it real
to them
* For Doctors, they can make better decisions from recommender systems and can use the logic
and data behind the recommender systems to gain patient trust

### Training a model to predict wolf v. husky ###
* Only one mistake -- do you trust the model?
* What if you're having a husky festival and one single wolf shows up and kills someone!?
* Upon further investigation, Carlos showed that the wolf predictor was actually a snow predictor

### Famous 20 Newsgroups dataset ###
* Most good predictions due to misleading features: email addresses, names
* Models can test well as far as training, test, and validation error, or whatever other
error benchmark you want to use, but how do we get intuitive trust
* Explanations help improve your model
* Mechanical Turk experiment
    * "In a few rounds of using a Mechanical Turk experiment, can I get as good or better
    performance than my 'gold standard' model"
    * Mechanical Turk lead to better results
* <a href="https://github.com/google/inception" target="_blank">Google Inception</a> -- explaining deep learning by showing the detected features that lead to a
prediction

### Game of Thrones Predictor ###
* Predicting whether characters will be alive or dead
* Carlos' model predicted that Ned Stark would be alive in Season 2
* Factors / features include
    * He's from the House of Stark
    * How many dead relatives does he have?

## Must-haves for ML in production ##
* Maximize resources, reuse features
* Never stop learning
* When scaling matters
* Explain yourself, explain your modlues
* Get away from the "my curve is better than your curve"

# DeepDive -- A Dark Data System #
**Professor Chris Ré, Carnegie Melon University**

## Ameliorating the Annotation Bottleneck ##
* Data programming, asynchronous deep learning
* non-"Dark Data" - spreadsheets, relational databases, etc.
* "Dark Data" - Valuable and hard to process data from scientific articles, documents, etc.
* Dark data processing was used for fighting human trafficking, partnering with law enforcement
agencies
* Some routine cases should be much easier
* Simple classifiers and regression
* Entity and relationship extraction
* **Challenge** - Make systems dramatically easier to use
* Willing to give up expressive power
* Non-CS PhD with a weekend to kill (i.e. hackathon)

## Rise of Automatic Feature Libraries ##
* Writing good features is painful
    * Many used default automatic feture libraries .. not great
    * Or deep learning
* Automatic feature libraries need large training sets, and creating such sets is expnsive
* ImageNet took years to build
* **Goal** - build large training sets using only standard tools

Note: I had to fix something that broke on my website at this point, so I stopped taking notes.

# Machine Learning for Analyzing Complex Time Series  #
**Emily Fox, Amazon Professor of Machine Learning, University of Washington**

* Hidden Markup Models (HMM)
* Challenges:
    * Sementation into behaviors
    * Parameter per behaviour
    * How many behaviors?
* Problem appears in many domains, including: Parsing EEG data, other medical diagnostic data, etc.
* Bayesian HMM's
* When you have time segments ~ 2 million, parsing the dataset every time you want to reatrain
your model becomes infeasible
* Solution: brak into manageable segments
    * Naive - analyze separately
    * Smart
    * Something else I didn't catch
* Runtime = 1 hr

## Discover structure across "space" ##
* Collection of time series where there are a sparse set of of dependencies
* Cluster regionas based on underlying price dynamics
    * Discover groups of tracts with correlated dynamics
    * In the housing example, look at locations that are closely related to the location you
    are trying to predict
    * Marrying trends / features that are globally applicable versus locally applicable
* Cluster and correlate multiple time series
    * Challenge: unknown cluster structure = unknown @ blocks and size of each
    * Solution: Latent factor model + Bayesian nonparametrics
* Methods for relating high-dimensional time series
    * Approach 1: Clusters of time series = marginal independence
    * Approach 2: Graphical models = conditional independence
        * Motivation for this came from Magnetoenchephalography (MEG) time-series data
        * Transform time-series from the time domain to the frequency domain
        * Example: daily returns from a set of global stock indicies (shows a node graph representing
        what countries are related?)
    * Approach 3: Low-dimensional embedding of dynamics
        * MEG word classification task
        * Helmet with 102 sensors
        * More accurate than any other current method

# Increasing Diversity in Data Science #
**Jennifer Rose, Professor @ Wesleyan**

## Why do we need diversity in data science? ##
* When it comes time to tell the story of your data analysis, your background introduces
 bias, and if you have too many similar individuals in the same place, that bias can carry
 more weight than it would be in a more diverse working group
* Diversity:
   * Impacts the questions we ask in the first place
   * Ramps up creativity

## Method for increasing diversity ##
* Strategies for increasing diversity
    * Branch out to other disciplines
    * Evoke curiosity and passion through multidisciplinary project-based learning
* Project-based learning
    * Base the learning approach on a real-world problem
    * Superior to traditional appraoches in promoting deep thinking, knowledge retention, etc.
* At Wesleyan, they used a multidisciplinary approach where the students pick a topic that
interests them, pick a dataset, and deelop a research question that the seek to answer
* Weeks 1 and 2
    * Generate testable research question
    * Conduct a literature review
* Weeks 3 and 4
    * Stats software basics (SAS, R, Stata, SPSS)
    * Data management
    * Descriptive stats and graphing
* Week 5 though Week 10
    * Conduct bivariable and multivariable analyses
* Week 11 through Week 14
    * Interpreting results and drawing conclusions
    * Reconciling study limitations
    * Presenting findings

* **Just in time learning**
* <a href="http://passiondrivenstatistics.com" target="_blank">passiondrivenstatistics.com</a>
* This method of teaching resulted in enrolling significantly more under-prepresented students
than the traditional math statistics course
* Coursera's data analysis and interpretation track

# Interesting Tools and Research #
* Apache Arrow
* Myria
* Non-python stats tools (SAS, SPSS, R <--- **nope**)
* MusicDB - longitudinal analysis of music
* SeaFlow (might be interesting for Mark)
* Ashes CAMHD (ocean floor HD cameras that pass photos to data scientists for analytics)
    * Separating foreground and background using matrix factorization

# Churn Prediction Tutorial #
***Note: I tried to put this tutorial on GitHub but had several issues with large files and right now it's not working. It'll be up sooner rather than later***

# Machine Learning in Production #
**Dr. Yucheng Low, Chief Architect @ Turi**

*Note: Arrived late to the conference today, and this was the first presentation I attended, so my notes
for this presentation are in complete.*

 * When using a classifier, one popular technique is to utilize geolocation
    * Example was identifying photos of pie in Seattle, where there's a very famous restaraunteur
    who has a reputable Coconut Cream Pie
    * So, if you were to utilize a geolocation of a user in Seattle, the  probability that
    their photo is a coconut crem pie is higher than if they were in Texas or something

# The Five Tribes of Machine Learning, and What you can Take from Each #
**Professor Pedro Domingo, University of Washington**

*This was a table, and I'll turn it in to a table tomorrow*

* Symbolists - Logic, Philosophy, Inverse deduction
* Connectionists - Neuroscience - Backpropgation
* Evolutionaries - Evolutionary Biology - Genetic programming
* Bayesians - Uncertainty - Probabilistic Inference
* Anologizers - Similarity - Kernal Machines

## Symbolists ##
* Learning is inverse deduction, i.e. induction
* Their focus is induction
* "If I know that Socrates is human, and humans are mortal, deduction dictates that Socrates is human"
* "If I know that Socrates is human, and I know that Socrates is mortal, what do I know about humans?"

## Connectionists ##
* The greatest machine laerning is the one inside your skull
* One connectionist believes that the way the brain learns can be defined in a single algorithm
    * Invented <a href="https://en.wikipedia.org/wiki/Backpropagation" target="_blank">Backpropogation</a>
    * Backprop works backwards from the last node in a decision trees, adjusting weights
    to reduce error for each node from end to beginning
* Google Cat Network - cats are the most uploaded singe video type on YouTube

## Evolutionaries ##
* Genetic algorithm of how evolution workds
    * Bitstring encodes a program that defines a person
    * Multiple permutations of bitstrings defines the biological difference between humans
    * The fittest bitstrings survive and reproduce themselves

## Bayesians ##
* What Bayesians are obsessed with is that all knowledge learned from data is uncertain
* Define probability of a hypothesis before you even see the data
* When you see the data, the likelihood, which then leads to the posterior probability
* Nearet Neightbor

## Analogizers ##
* Kernel Machines = Support Vector Machines
* Support Vector Machines - giving a wide birth between two categories for an output of a classifier

## Putting the Pieces Together ##
* Representation
    * Probabilistic Logic (e.g. Markov logic networkds)
    * Weighted formulas -> distribution over states
* Evaluation
    * Posterior probability
    * User-defined objective function
* Optimization
    * Formula discovery: Genetic programming
    * Weight learning: Backpropogation

## What a Universal Learner Will Enable ##
* Home robots
* World-wide brains
* Cancer cures
* 360 degree recommenders

# Small Team, Large Impact - how we solved it #

**Robert Glinton, Senior Director of Data Science Applications, Salesforce**

* Increasing productivity by building tools for data prep, experimentation, and deployment
* Time wasted when data pipeline is not durable
    * 3-6 weeks to find data
    * 1 week to understand the data
    * Some more time to evaluate and write scrips
* <a href="https://alation.com/" target="_blank">Alation - automated data catalog</a>
    * Automatically crawls and documents data across all data sources to understand the emantics of data
    * Users write articles and have conversations around artifacts identifiied by Alation
* Wpeedup to data preparation pipeline
    * Faster onboarding
    * Correct data
    * Alignment to semantics
* Feature selection automation
    * orgDNA - explors cubes in the data and scores them for how unusual the output is relative to the baseline
    * In this example Promo = Promo II is a good feature because revenue $1,000 is unusually small
    compared to baseline of $1M
        * Because it explains the varience in revenue
    * Computationally they pull this off by constraining the models to no more than three dimensions
* <a href="https://www.dominodatalab.com/" target="_blank">Domino - continuous experimentation system for data science</a>
    * Version the data the code is analyzing in addition to the code itself
    * Dockerized backend to tweak experiments endlessly
        * AKA "Versioned experiment"
        * Allows for apples to apples comparison between models
        * Docker + Reproducibility = Many ideas in parallel
* Why not Github?
    * Support for experiments
    * Reproducibility
    * Two other things I didn't catch
* Deployment
    * Model monitoring automation - design a monitoring system that will alert you when it detects
    drift or other metrics that warrant attention to a model
    * ***This will be useful for the btc price predictor I'm working on***

# Industry Panel with representatives from Quora, Turi, Tableau, and Pinterist #
**Dr. Xavier Amatrian from Quora**
**Dr. Carlos Guestrin from Turi**
**Dr. Jock MacKinlay from Tableau**
**Dr. Jure Leskovec from Pinterist**
**Richard Waters as Moderator**

* Measuring model improvement
    * Carlos: it depends on the application -- what does a 1% improvement in accuracy mean
    for Fraud vs. some classifier
    * Any improvement in fraud detection is positive. Fraud is just flushing money down the
    toilet, so any reduction in fraud is a good thing
* Building machine learning applications and models so they are more widely applicable to
people in your company
* Need to always think critically about what the business questions being asked are and how
they can be generalized to make a model more widely useable
* **Jock**: Need to identify low hanging fruit versus harder problems
* **Carlos**: Brought up HR wanting to apply machine learning to identifying the best interview
candidates
* **Jure**: Have to identify / demonstrate value early on by hacking your models together quickly
* **Xavier** Occam's Razor -- in the first stage, don't even introduce machine learning. Let
the product team hard code something that conforms with the business rules of the company.
Deep learning has great applications in image and language processing, but there are many
applications where deep learning would likely yeild lower performance than simpler models
    * A 1% increase in accuracy might not be worth the increase in complexity associated with
    deep learning. Additionally, you must also consider the amount of maintenance that would
    be needed to maintian a deep learning model in production
* **Carlos**: one space that is really aided by machine learning is streaming / activity data
(user activity on a website, for example). Boosted Trees is useful in this space
* **Jure**: always think about the value of your model / application at its earliest iteration?
Think about providing some accuracy over the baseline and how useful that would be BEFORE you
go down the road of optimization
* **Carlos:** Transparency is one of the most impotrant things in machine learning. Can you explain
why certain recommendations are happening? If not, **you've got a problem.**
* **Jock**: An issue greater than bug squashing is making sure all your assumptions are correct

**Question: is deep learning over-hyped right now?**

* **Carlos**: "I think deep learning is definitely at the top of the hype cycle"

**Question: what are some of the worst bugs you've experienced?**

* Didn't use the right notion of positive in a label -- needed better thinking on what "positive" is
* At least 90% of the things you do should fail. If that isn't the case, you aren't moving fast
enough or need to be comfortable with taking more risks
* In terms of targeting users to return to a website, are you measuring the success of a particular
email, or is the result of sending an additional email just because you have sent 6 emails
instead of 5
* Even if your model looks good, it may not look good for the right reasons

**Question: How do you trust your models?**

* Develop a hypothesis before your start building a model
* Multiple hypothesis testing problem -- even a bad model can be right once a day

**Question: do we need a learning layer on top of your machine learning models to evaluate performance?**

* Yes
* Still problems with overfitting and multiple hypothesis testing
* *Ridge regression, l1 and l2 norm optimization, functional programming to call multiple
types with the same data helps in this regard?*

**Question: How do you balance the amount of research versus product work?**

* Tableau has a hackathon culture
* Make sure enough of the research portfolio has impact on the rest of the organization
* See what problems really matter for research and what problems have the most impact on the company

**Question: Do I need to have a PhD to get in to machine learning?**

* 10-15 years ago you would not get machine learning in a standard computer science or statistics program.
That is changing rapidly

**Question: HR example**

* We can miss opportunities and waste resources -- got to find a happy medium
* There's a company in Seattle that automates the first-round phone interview. You interview
with a robot
* When you hire someone, you only have data for the people who you hired. You don't have data on
the people you rejected

**Question: I have billions of records in my data, and not many computing resources. How do I sample this data
to label it and run experiments**

* Take advantage all the unlabeled data in a semi-unsupervised machine learning model
