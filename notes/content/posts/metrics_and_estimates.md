+++
title = "Metrics and estimates"
date = 2018-07-31T18:47:18+02:00
tags = ["statistics"]
categories = ["statistics"]
draft = false
+++

## Mean

Most basic estimate for location.
As simple as possible it is just and average value from a list of values described with equation:

$$ \frac{\sum\limits_{i=1}^n x_i}{n} $$


## Trimmed Mean

It is a small variation of mean. Fist Values are sorted, then some number of values are dropped from colection beginning and end. After that normal avearge value is computed. It is Very usefull when we want to eliminate the influence of extream values in dataset.

## Weighted Mean

It is calculated by multiplying every value by its weight and deviding their sum by the sum of weights. See the formula:

$$ \frac{\sum\limits_{i=1}^n w_ix_i}{\sum w_i} $$


It is very usefull when you want to distinct  that some values are more important than others.

When dataset has data that represents some different groups and measuring them equally would not reflect real world example.

## Median

Basically it is a middle value on a sorted list of values. 
If there is an even number of values then meadin is computed from avearge of two middle values.

## Weighted Median

It is computed that there is a half weights above and below median value.
First data needds to be sorted then insted of picking middle value we pick such a value that half of the weights are above this value and other half is below.

## Trimmed Mean

This value is computed by droping some fixed number of values from the beginning and from the end of sorted dataset. Trimmed mean eliminates the influence of extreme values.

## Outlier

Extream cases that exist in dataset. Outlier for example is every value that is very distant from any other value in dataset.

## Robust estimate

Estimate that is not influenced by outliers. We can call median a robust estimate cause it does not care about outliers in the datase, computing median omits extreme values.
