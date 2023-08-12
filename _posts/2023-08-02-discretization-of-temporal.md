---
layout: distill
title: Discretization of Temporal Data 
description: What is the Discretization?
tags: timeseries, discretization 
giscus_comments: true
date: 2023-08-02
featured: true
img: "/assets/posts/discretization/cut.png"
authors:
  - name: Jungmin Kim
    affiliations:
      name: KAIST

bibliography: all.bib

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }
---

## What is the Discretization?

Discretization is the process of transforming continuous space valued series into a discrete values series. Continuous space valued series $$X= \{x_1, x_2, ..., x_n\}$$ into a discrete valued series $$Y=\{y_1, y_2, ..., y_n\}$$. Temporal discretization refers to the discretization of time series data, a preprocessing step in transforming the temporal data into timely intervals.
The main part of the discretization process is choosing the best cut points which split the continuous value range into discrete number of bins usually referred to as states.

## How does discretized input help TS model?
According to Rabasnar et al.<d-cite key="rabanser2020effectiveness">, *data-binning*<d-footnote> data-binning converts real-valued time series into categorical ones. </d-footnote> can help timeseries models. 

* The time-series data are strongly influenced by negative elements such as noise, missing value, inconsistency, and unnecessary data.
* Converting time series to time intervals shows less representation of time series, which can lead to reduced and simplified data, faster training, more accurate, compact, and shorter results, and possibly reduced data distortions.


## Symbolic Representation
Symbolic representation<d-cite key="lin2003symbolic"/> refers to the use of symbols, signs, or abstract representations to convey ideas, concepts, or information. It is a fundamental aspect of human cognition and communication. Symbols can be in the form of words, numbers, images, gestures, or any other system of representation that carries meaning such as Languages, Mathematics, Visual arts, …


### Advantages of Symbolic Representation 

1. Exploiting the richness of data
2. Lower bounding properties which Guarantee that the distance between times series in discrete space is less or equal to the distance in original space
3. Reducing the dimensionality

### Symbolic Aggregate Approximation (SAX)
 
#### Step 1. 

First, Transform the data into the Piecewise Aggregation Approximation (PAA)



<figure>
<center>
<img src="/assets/posts/discretization/comparison.png" style="width:70%">
</center>
<figcaption>
</figcaption>

</figure>

#### Step 2. 
 
 An intermediate representation between raw time series and the symbolic strings.


 <figure>
<center>
<img src="/assets/posts/discretization/paa.png" style="width:70%">
</center>
<figcaption>
</figcaption>
</figure>


####  Step 3. Next, 

Symbolizing the PAA representation into a discrete string.

 <figure>
<center>
<img src="/assets/posts/discretization/cut.png" style="width:70%">
</center>
<figcaption>
</figcaption>
</figure>


