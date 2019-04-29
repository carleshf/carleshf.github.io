---
published: false
category: blog
layout: post
title: Understanding linear regression models
---

During the last years, I've been working a large part of my time as data analysis. First during my Ph.D. studies at the [Barcelona Institute for Global Health](https://www.isglobal.org/en) and now as a research fellow at the [Boston Children's Hospital](www.childrenshospital.org/).

Interestingly, the main tool I've been using during these 5 years are __linear regression models__.  In this article, I will go through an overview of how __linear regression models__ works while implementing them (although I know that there are very efficient implementations of them in python, R, or octave).

## Introduction

The __linear regression models__, from now on __linear regression__, describe a continuous _response_ variable (also known as _dependent_ variable) as a function of one or more _explanatory_ variables (or _independent_ variables). The case where we have a single explanatory variable is called __simple linear regression__ and when we have more than one explanatory variable is called __multiple linear regression__.

### The equation

In __linear regression__, the relationships are modeled using __linear predictor functions__ whose unknown model parameters are estimated from the data. Such models are called __linear models__ and their general equation is:

$$
   y = \beta_{0} + \sum \beta_{i} X_{i} + \epsilon_{i}, i \in 1,...,n
$$

In simple linear regression `i` will only take `1` while in multiple linear regression `i` will take values from `1` to `n`. 

## Simple linear regression

Let's explain the linear regression equation in simple linear regression, where we have a _dependent_ variable (`y`) and a single _independent_ variable (`x`)

$$
   y = \beta_{0} + \beta_{1} x + \epsilon
$$

In the univariate approach, the model that defines the relationship between the _dependent_ variable and the _independent_ variable has a single term, therefore it defines a linear relationship between them.

Let’s visualize this concept in a real data-set. We start loading and the _iris_ data-set, subset only two features (`sepal_length` and `petal_width`), and plot them.

[python code]

We want to create a model, a linear model, to explain the relationship between `sepal_length` and `petal_width`. To do so, we will need to find the proper values for $\beta_{0}$ and for $\beta_{1}$ that will define the line across the cloud of points:

[python code]


