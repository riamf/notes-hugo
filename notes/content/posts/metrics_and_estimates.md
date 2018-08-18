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

## Examples

To perform examples I will use [Airbnb publicly available dataset](http://tomslee.net/airbnb-data-collection-get-the-data). I picked dataset that contains rooms availabe for rent in Warsaw and cost (in US$) per night. This is how the dataset looks like:

| room_id | price |
|---------|-------|
| 1       | 110.0 |
| 2       | 118.0 |
| 3       | 72.0  |

For computation I will use python3 and to load data I want to go with `pandas` framework cause if is extreamly easy to use `DataFrame` to load and do any data manipulation with it.

### Mean

{{< highlight python "linenos=inline,hl_lines=8 15-17,linenostart=0" >}}
    import pandas as pd

    data = pd.read_csv('./data/airbnb_warsaw_2017.csv')
    n = len(data)
    mean_price = sum(data.price)/n
    # 54.68495536686262
{{< / highlight >}}

### Trimmed Mean

{{< highlight python "linenos=inline,hl_lines=8 15-17,linenostart=0" >}}
    sorted_price = sorted(data.price)
    trim_size = int(0.1 * len(data))
    trimed_price = sorted_price[trim_size:(len(data) - trim_size)]
    trimmed_mean = sum(trimed_price)/len(trimed_price)
    trimmed_mean
    # 45.97714285714286
{{< / highlight >}}

### Median

{{< highlight python "linenos=inline,hl_lines=8 15-17,linenostart=0" >}}
    size = len(sorted_price)
    # 4593
    median_index = math.ceil(size/2)
    sorted_price[median_index]
    # 42.0
{{< / highlight >}}

### Weighted Mean

This dataset has three types of rooms available, lets create weights mapping for them:

{{< highlight python "linenos=inline,hl_lines=8 15-17,linenostart=0" >}}
    mapping = {"Entire home/apt": 3, "Private room" :2, "Shared room": 1}
    data['weight'] = data.room_type.map(lambda x: mapping[x])
{{< / highlight >}}

This means that most important for us is type called `Entire home/apt`.

Now lets calculate weighted mean for price:

{{< highlight python "linenos=inline,hl_lines=8 15-17,linenostart=0" >}}
    weighted_mean = sum(data.weight * data.price)/sum(data.weight)
    # 57.14528152260111
{{< / highlight >}}

### Outlier 

{{< highlight python "linenos=inline,hl_lines=8 15-17,linenostart=0" >}}
    sorted_price[size-1]
    # 3000
    sorted_price[0]
    # 3
{{< / highlight >}}

## Conclusion

We can clearly see that this values differ but this difference is not a huge one, it always depends on data.
Basic metric for location is mean but as it can be seen in abowe example it can be sensitive to extreme values.
Other metrics like `median` or `tremmed mean` are more robust.

