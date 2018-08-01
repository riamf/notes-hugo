+++
title = "Metrics_and_estimates"
date = 2018-07-31T18:47:18+02:00
tags = ["statistics"]
categories = ["statistics"]
draft = false
+++

# Metrics and estimates

## Location Estimates
### Mean

    Most basic estimate for location.
    As simple as possible it is just and average value from a list of values described with equation:

    ![Example image](/images/mean.png)

#### Trimmed Mean

    It is a small variation of mean. Fist Values are sorted, then some number of values are dropped from colection beginning and end. After that normal avearge value is computed. It is Very usefull when we want to eliminate the influence of extream values in dataset.

### Weighted Mean

    It is calculated by multiplying every value by its weight and deviding their sum by the sum of weights. See the formula:

    ![Example image](/images/w_mean.png)

    It is very usefull when you want to distinct  that some values are more important than others.

    When dataset has data that represents some different groups and measuring them equally would not reflect real world example.

### Median

    Basically it is a middle value on a sorted list of values. 
    If there is an even number of values then meadin is computed from avearge of two middle values.

### Weighted Median

    It is computed that there is a half weights above and below median value.
    First data needds to be sorted then insted of picking middle value we pick such a value that half of the weights are above this value and other half is below.

### Trimmed Mean

### Robust

### Outlier

