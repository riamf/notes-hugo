+++
title = "Estimates of variability"
date = 2018-08-18T16:08:06+02:00
tags = ["statistics"]
categories = ["statistics"]
draft = false
+++

## Deviations

The most widely used estimates are based on differences in data, deviations, like observation and given estimate of location. Take for example this set `{1,2,3}`. Mean equals `2` and deviations are:

    1. 2 - 1 = 1
    2. 2 - 2 = 0
    3. 2 - 3 = -1

This deviations tell us how dispersed observations are around the central value. What is also interesting is that the sum od deviations from the mean value equals 0.

## Mean absolute deviation 

So how to get some information from deviations. We can take absolute value from every deviation here it would be sum from set of `{1,0,1} = 2` and divide it by number of observations so it is just like counting mean of absolute deviations
`2/3 = 0.6666`

## Variance & Standard deviation

Probably the best known estimates of variability. Variance is an avarge of squared deviations computed with equation

$$ s^2 =  \frac{\sum(x - \bar{x})^2}{n-1} $$
$$ s = \sqrt{s^2} $$

Standard deviation is easier to interpret since it is on the same scale as the original data set.

## Degrees of freedom 

I always wonder why there is `n-1` used in some statistical equations. Most of the time `n` is so large that it does not matter that there is minus one there cause it has really low influence. Nonetheless most of the time you want to make estimate about the population based on a sample so if you use whole `n` you will underestimate. This is referred to as a biased estimate but if instead you divide by `n-1` then the estimate becomes unbiased. Number of degrees of freedom takes into account number of constraints in computing estimate. So for example when computing standard deviation it depends on computing mean value and that is one constraint so there are `n-1` degrees of freedom left.

## Median absolute deviation

Variance, standard deviation or mean absolute deviation, neither of them is robust to outliers. But as before, here we can also compute median using the equation:

$$ Median(\left | x_1-m \right |, \left | x_2-m \right |...\left | x_N-m \right |) $$

Where `m` is observations median value. It is called `MAD` of simply median absolute deviation and is not influenced to outliers.

