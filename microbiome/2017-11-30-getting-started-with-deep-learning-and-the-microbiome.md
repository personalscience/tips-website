---
title: Deep learning and the microbiome
custom_order: 8
#date: '2018-05-18'
slug: getting-started-with-deep-learning-and-the-microbiome
image: microbiome-assets/streptococcus_bacteria_mixed_with_bifidobacterium__6.jpeg
description: Summary of links about how to apply AI to studying the microbiome.
categories:
  - AI
  - microbiome
---
One of the toughest parts about exploring the microbiome is figuring out what to explore. Approaching a subject this new and complicated, we often don't even know which questions are worth asking. It's tempting to suspect we can find simple generalizations that explain aspects of health or well-being, like the Firmicutes/Bacteroidetes ratio that was once thought to correlate and possibly drive obesity, or enterotypes, the idea that people can be classified into predictable categories of microbial hosting. Many, perhaps all of these approaches that seemed promising at first turn out to be more complicated upon followup.

The one thing we have lots of is data. Can we turn that into an advantage?

Machine learning is a set of techniques that try to take a large body of data and automatically calculate a model that can be used to extrapolate new information, either about the existing data, or about likely scenarios for new data.

For understanding the microbiome, this means: take a large number of samples and figure out their characteristics in some way that lets us see new attributes and perhaps project understanding on new samples.

* Predict likely attributes for new samples (diet type, possible disease state)

* Summarize the data in new and interesting ways, by clustering similar samples together or indicating similarities and differences that we might not have expected.

With Deep Learning, I'd like go one step further and find interesting attributes that I don't know to look for.  Here are some examples:

* Find some rare microbes, or rare combinations of microbes, and find what their hosts have in common. For example _Anaeroplasma_, which I've found anecdotely in an unusually high proportion among people who have lived in Europe.
    
## How it works

Think of a microbiome sample as a large vector of attributes

* Metadata that describes the host. 
    * A questionnaire (survey)
    * General private information (e.g. age, gender, etc.)
* Taxa 
    * names
    * abundance information
    
## Introductory reading
If you know nothing at all, I would start with the following easy-to-read articles:

[Machine Learning Is Fun! (parts 1 through 8)](https://medium.com/@ageitgey/machine-learning-is-fun-80ea3ec3c471)  A very readable and motivating introduction to how it all works. Using step-by-step examples, the author leads through the techniques in a way that any programmer will leave thinking "I can do that!" and non-programmers will have enough background to feel that they can pick out their own scenarios where machine (and deep) learning would be useful.

[A Non-Technical Guide to Machine Learning and Artificial Intelligence](https://medium.com/machine-learnings/a-humans-guide-to-machine-learning-e179f43b67a0) is another easy read, but with less of the programming side. Lots of links to other sources.

## Go deep with the original papers

[Github: Deep Learning Papers Reading Roadmap](https://github.com/songrotek/Deep-Learning-Papers-Reading-Roadmap): wonderful Github repository of top academic papers, organized by topic and ranked with stars.  Includes the Python script download.py to let you easily download all links on that page.

## Academic research applying deep learning to the Microbiome
I couldn't find much that directly applies DL ideas to understanding the microbiome. Although most labs these days have tinkered with various machine learning algorithms to cluster results somehow, I haven't seen many summaries for how you might, for example, classify healthy or unheathy microbiomes. This could be -- probably is -- because the task is impossible given current data, or it could be that the right clever techniques haven't yet been discovered.

[Deep Learning Tools for Human Microbiome Big Data (2018) ](https://link.springer.com/chapter/10.1007/978-3-319-62521-8_21) A Romanian group used the  WEKA tools to classify subsets of the Human Microbiome Project dataset.

["Deep Learning in Bioinformatics"](https://academic.oup.com/bib/article-abstract/18/5/851/2562808) is a 2016 summary of the methods and how they are currently being applied to bioinformatics. There is nothing specific to microbiome research, but any bioinformatician will quickly see how it can be made applicable.

[Ali Faruqi posted to Github](http://alifar76.github.io/posts/) some of his explorations with machine learning and the microbiome.

[Regularization Learning Networks](https://arxiv.org/abs/1805.06440) A math-heavy argument for why Regularization Learning Networks (RLNs) outperform other deep learning methods when applied to the tabular data types, with examples using microbiome data.