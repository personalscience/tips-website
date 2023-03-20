---
title: Sonification of the microbiome
custom_order: 7
# date: '2018-05-05'
description: Listen to a year's worth of my microbiome results.
image: microbiome-assets/photorealistic_bacteria_wearing_headphones_blue_tint___in_the_style_of_a_scientific_drawing_4.jpg
slug: sonification-of-the-microbiome
categories:
  - microbiome
  - R
---


A graphic visualization is an effective way to communicate a data set, but we humans have other, non-visual senses too. Generating sound from a data set, also known as *sonificiation*, is a well-studied problem, but I wanted to know how to a do a very simple version in R.

The best overall introduction to data sonification is at [Programming Historian](https://programminghistorian.org/lessons/sonification), which surveys the basic principles and offers a step-by-step working guide, including sample code for a few Python libraries. The instructions also point to [Musicalgorithms](http://musicalgorithms.org/4.1/app/), a web app version that lets you upload data and hear your sonification, no programming required. I couldn't quite get the site to work on my quick try, but it might be worth more effort if you don't have time to dig further into programming.

Sound synthesis packages
------------------------

There are numerous sound-related packages in R. These are the main ones that appear to do what I want

[tuneR](https://cran.r-project.org/web/packages/tuneR/index.html) is the one I eventually went with for the example below. It's been regularly maintained, including updates from 2022.

[Seewave](http://rug.mnhn.fr/seewave/) semes to be a recent package under active development (most recent version is July 2021). There is a nice [I/O of Sound](https://cran.r-project.org/web/packages/seewave/vignettes/seewave_IO.pdf) PDF that explains how to import and export sound, but I couldn't find a quick-and-dirty example of sound synthesis.

[Sound generation with soundgen](https://cran.r-project.org/web/packages/soundgen/vignettes/sound_generation.html) (2022): lengthy documentation of a package intended for the synthesis of animal vocalizations, including human non-linguistic vocalizations like sighs, moans, screams, etc. It can also create non-biological sounds that require precise control over spectral and temporal modulations, such as special sound effects in computer games or acoustic stimuli for scientific experiments.

[playitbyr](http://playitbyr.org/index.html) is a package that’s exactly what I want: allows you to listen to a data.frame in R by mapping columns onto sonic parameters. Unfortunately it doesn’t seem to work with R 3.4+

[Sonify](https://cran.r-project.org/web/packages/sonify/sonify.pdf) (2017) looks good too, but I didn't have time to try it. It hasn't been updated in a while.

Tutorials
---------

[TuneR tutorial](https://www.r-bloggers.com/data-sonification-with-r-the-sound-of-twitter-data/): good example sonifying tweets. I couldn't get this one to work, but you'll see that it formed the basis of my working example below.

[Sonification of Iris data](https://www.r-bloggers.com/data-sonification-with-r-the-sound-of-twitter-data/) (2015): a step-by-step example using the most famous data science data set.

[Play Happy Birthday in R](https://stackoverflow.com/a/31783039/2407580) a Stackoverflow answer gives a short code segment, easy to understand and works great. But it uses the more limited `audio` package, which isn't quite as advanced as the others.

If you want a much more detailed treatment, get the 600-page [Handbook of Sonification](http://sonification.de/handbook/index.php/chapters/), edited by Thomas Hermann (2011). The website lets you download each chapter, or the [whole book](http://sonification.de/handbook/download/TheSonificationHandbook-HermannHuntNeuhoff-2011.pdf) as a PDF.

A Simple Sonification Example in R
----------------------------------

Start with the `TuneR` package.

On a Mac, I set the output to the Audio File Play `afplay` utility, which is built into OSX.

``` r
library(tuneR)
tuneR::setWavPlayer("/usr/bin/afplay")
```

I'm going to sonify some microbiome data. I have five rows of microbes, with percentage abundances of each at the various sampling dates (columns). My data has hundreds of columns like this.

|                  |  2014-05-16|  2014-10-17|  2015-02-24|  2015-04-21|  2015-04-28|
|------------------|-----------:|-----------:|-----------:|-----------:|-----------:|
| Bifidobacterium  |       33.31|        6.11|        0.68|        0.84|        0.16|
| Faecalibacterium |       15.09|        3.70|       22.86|       20.79|       18.00|
| Bacteroides      |        7.34|       29.17|       20.38|       10.64|       18.57|
| Akkermansia      |        6.86|        0.85|        0.00|        1.42|        1.08|
| Roseburia        |        5.91|       15.30|        8.45|        9.99|        9.74|

Now we generate the sound. I admit the following is some pretty ugly code, but it works. I didn't have time to figure out how to optimize it. Would you like to help?

The basic idea is to start with a single instant of sound (the call to `silence()` below) and then loop one-by-one through each of the columns in my microbiome data frame, `bind`ing the sound units together. I generate five separate sine waves at each instant, each representing the tone of the microbiome abundance at that point in time.

The end result must go through the `normalize()` function in order to preserve a standard bit depth, and then we can play it.

``` r
sound.length <- 0.05
sampling.rate <- 3000
bits <- 32

long.pause <- 0.5 # in seconds

w <- tuneR::silence(duration = long.pause, xunit = c("samples", "time")[2], bit=bits, samp.rate=sampling.rate)

for(i in 1:length(microbiome_data)){
  wobj <- tuneR::sine(microbiome_data[1,i], duration=sound.length, xunit = c("samples", "time")[2], bit=bits, samp.rate=sampling.rate) +
    tuneR::sine(microbiome_data[2,i], duration=sound.length, xunit = c("samples", "time")[2], bit=bits, samp.rate=sampling.rate) +
    tuneR::sine(microbiome_data[3,i], duration=sound.length, xunit = c("samples", "time")[2], bit=bits, samp.rate=sampling.rate) +
    tuneR::sine(microbiome_data[4,i], duration=sound.length, xunit = c("samples", "time")[2], bit=bits, samp.rate=sampling.rate) +
    tuneR::sine(microbiome_data[5,i], duration=sound.length, xunit = c("samples", "time")[2], bit=bits, samp.rate=sampling.rate) 
  #cat(i)
  w <- tuneR::bind(wobj,w)
}

w <- tuneR::normalize(w,unit = "32")
tuneR::play(w)

# writeWave(w,"microbiome5.wav”)
```

The final result can be saved as a local file, which I uploaded to Soundcloud and you can listen to here:

<iframe width="100%" height="300" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/440924946&amp;color=%23ff5500&amp;auto_play=false&amp;hide_related=false&amp;show_comments=true&amp;show_user=true&amp;show_reposts=false&amp;show_teaser=true&amp;visual=true">
</iframe>
