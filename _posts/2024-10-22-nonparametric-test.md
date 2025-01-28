---
layout: post
title:  "Understanding Alzheimer's Diagnosis Predictors"
date: 2024-10-20
description: The data used in this post looks at numerous medical and lifestyle records
  for 2,149 unique patients with and without diagnosed Alzheimer's disease. This analysis will first determine the distribution of Mini-Mental State Exam data for patients with and without a family history of Alzheimer's. Afterwards, a Mann-Whitney test will show that there is no significant difference between the two groups. Finally, conclusions and future study recommendations will be made based on the results of this post's study. 
image: "/assets/img/435_report1/brain.png"
display_image: false  # change this to true to display the image below the banner 
---

# Setting the Stage

***

### Motivation

<p class="intro"><span class="dropcap">A</span>lzheimer's disease is a global challenge. This neurodegenerative disorder, responsible for 60-80% of dementia cases, profoundly affects memory and cognitive function. While researchers and medical professionals work tirelessly to unravel its mysteries, tools like the Mini-Mental State Exam (MMSE) help track cognitive decline.

The MMSE is a 30-point questionnaire that measures cognitive skills like memory, orientation, and attention. A low score means severe impairment, but MMSE scores alone aren’t enough for a full diagnosis. Still, they’re invaluable for monitoring progress and tailoring care plans.

### Scientific Question

Is there is a significant difference in cognitive function (measured by MMSE scores) between patients with and without a family history of Alzheimer's disease?

### Statistical Hypotheses

We’re testing whether family history impacts MMSE scores. Here’s how we set up the hypotheses:

- Null Hypothesis ($H_0$): There’s no difference in MMSE scores between the two groups.

- Alternative Hypothesis ($H_A$): There’s a significant difference in MMSE scores between the two groups.

With an alpha level of 0.05, let’s dive into the data to see what it reveals.

# Getting to Know the Data

***

## Data Overview

The MMSE data separated by family history, was pulled from [kaggle.com Rabie El Kharoua (2024)](https://www.kaggle.com/datasets/rabieelkharoua/alzheimers-disease-dataset). The group of patients with a family history of Alzheimer's contains 542 unique observations, while the other contains 1607 observations. The variable MMSE has scores ranging from 0-30, with the lower scores indicating a higher level of cognitive impairment. 

### Summary Statistics for Two Samples

Let’s start with the basics. Here’s a snapshot of how MMSE scores stack up for both groups:

| Family History Alzheimers | Mean | Median | Standard Deviation | Minimum | Maximum | First Quartile | Third Quartile | n |
| ----------- | ----------- | -----|--------|--------------------|---------|---------|----------------|----------------|---|
| No    |  14.68   | 14.20 | 8.59 | 0.01 | 29.99 | 7.13 | 22.09 | 1607 |
| Yes   |  14.99  | 15.29 | 8.70 | 0.05 | 29.97 | 7.26 | 22.60 | 542 | 

<figure>
  <figcaption>Table 1: Summary Statistics</figcaption>
</figure>

The averages look similar, but let’s dig deeper.


### Check for Normality

#### 1. Visualizing the Distributions

Using histograms and Q-Q plots, we explored the distribution of MMSE scores for both groups. Spoiler alert: it’s not normal.

<figure>
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/qqplots.png" alt="Description of image" style="width:600px;height:350px;">
  <figcaption>Figure 1: Normal QQ Plots</figcaption>
</figure>

<figure>
<div style="display: flex;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/yes_hist.png" alt="Image 1" style="width: 45%; margin-right: 10px;" />

  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/no_hist.png" alt="Image 2" style="width: 45%;" />
</div>
<figcaption>Figure 2: Sid-by-Side MMSE Score Histograms</figcaption>
</figure>

#### 2. Shapiro-Wilk Test

For a formal check, we ran a Shapiro-Wilk test:


| Shapiro-Wilk Normality Test |
| ------------------------------|
| W  Statistic | p-value  |
| 0.95262 | < 2.2e-16 |

<figure>
 <figcaption>Table 2: MMSE Scores for Patients Without a Family History of Alzheimer's</figcaption>
</figure>

| Shapiro-Wilk Normality Test|
| ------------------------------|
| W  Statistic | p-value  |
| 0.0.95105 | 2.05e-12 |

<figure>
 <figcaption>Table 3: MMSE Scores for Patients With a Family History of Alzheimer's </figcaption>
</figure>

Since both p-values are below 0.05, we concluded that the data isn’t normally distributed.


### Check for Equal Variance

An F-test showed that the variances between the two groups are roughly equal (p-value = 0.6977), so we’re good to go on that front.


# Non-Parametric Tests for Significance

***

### **1. Mann-Whitney Test**

Given the non-normal data, we used the Mann-Whitney test to compare the two groups. This test ranks the data rather than relying on raw scores, making it robust for non-parametric situations.

Result:

- p-value: 0.4949

- Conclusion: No significant difference in MMSE scores between groups.


### **2. Permutations and Randomized Combinations**

To further test the significance of our results, we used permutation and randomized combination methods. By shuffling the data 1,000 times and recalculating the test statistic, we created a distribution of possible outcomes under the null hypothesis.

Both methods confirmed the findings of the Mann-Whitney test: the observed differences in MMSE scores are likely due to random chance.

$H_0: \mu_{Family History} = \mu_{NoFamilyHistory}$

*"There is no significant difference in MMSE scores between individuals with a family history of Alzheimer's and those without."*

$H_A: \mu_{Family History}\ne \mu_{NoFamilyHistory}$

*"There is a significant difference in MMSE scores between individuals with a family history of Alzheimer's and those without."*

### Explanation of Techniques

A permutation test calculates a test statistic multiple times after randomly rearranging the data points to either group. For this data, that means randomly assigning MMSE scores to either participants with a family history of Alzheimer's, or without a family history of Alzheimer's. This results in a distribution of possible outcomes that could have been observed if the groupings were due to chance. For this study, 1,000 permutations were simulated, calculating the difference in sample means each time to test if the difference was greater than the observed difference in our original dataset.

In this context, combinations describe the reshuffling of data groupings in a similar manner to the permutations test, but we are sampling without replacement. This method does not enumerate all possible outcomes, but still provides insight into whether the observed differences in the original data are statistically significant.

Both the permutation and randomized combinations approaches support that our data follows what we would expect. The permutations and combinations density plots below illustrate the distributions of W statistics generated from 1,000 permutations and combinations. The original Mann-Whitney W statistic represented by the blue dashed line lies within 95% of both distributions, suggesting there is not a deviation from the expected outcome if the two family history groups were similar. 


# Concluding Remarks

***

So, what did we learn?

- The data isn’t normally distributed, so non-parametric tests were necessary.

- Across all tests, we found no significant difference in MMSE scores between individuals with and without a family history of Alzheimer’s.

Does this mean family history doesn’t matter? Not necessarily. Cognitive function is influenced by a web of factors, and MMSE scores are just one piece of the puzzle.



# Recommendations for Future Studies

***

While this analysis didn’t uncover significant differences, there’s still plenty of potential in the dataset. Here are some ideas for future research:

1. Predictive Modeling: Can features like age, education, or diagnosis predict MMSE scores?

2. Subgroup Analysis: Are there hidden patterns within specific age groups or ethnicities?

3. Longitudinal Studies: How does family history influence cognitive decline over time?

Alzheimer’s research is a complex but essential field, and every study—whether it finds a significant result or not—helps us get closer to understanding and combating this disease.
