---
layout: post
title:  "Nonparametric T Test on Alzheimer's Disease Data"
date: 2024-10-20
description: The data used in this paper looks at numerous medical and lifestyle records
  for 2,149 unique patients with and without diagnosed Alzheimer's disease. This analysis will first determine the distribution of Mini-Mental State Exam data for patients with and without a family history of Alzheimer\'s. Afterwards, a Mann-Whitney test will show that there is no significant difference between the two groups. Finally, conclusions and future study recommendations will be made based on the results of this paper\'s study. 
image: "/assets/img/LA_blog.jpg"
display_image: false  # change this to true to display the image below the banner 
---

# Introduction

Alzheimer's disease is a neuro-degenerative disorder that affects people around the world, causing memory loss and a severe decline in cognitive function. A widely used tool for assessing the cognitive function of Alzheimer's patients is the Mini-Mental State Exam (MMSE), which allows us insight into the severity of the participant's cognitive level.

### Scientific Question

This paper aims to investigate whether there is a significant difference in cognitive function (measured by MMSE scores) between patients with and without a family history of Alzheimer's disease.

### Statistical Hypotheses

The hypotheses for this paper will test if there is a significant difference in cognitive function score via the MMSE between our two groups. The null and alternative hypotheses will be as follows, with a chosen alpha level of 0.05.

Null: $H_0:\mu_{Family History} = \mu_{NoFamilyHistory}$

Alternative: $H_a: \mu_{Family History}\ne \mu_{NoFamilyHistory}$

$$
\begin{aligned}
H_0:\mu_{Family History} = \mu_{NoFamilyHistory}
H_a: \mu_{Family History}\ne \mu_{NoFamilyHistory}
\end{aligned}
$$

### Procedure Used/Methodology

This procedure will start by grouping participants by their status: having or not having a family history of Alzheimer's disease. Some preliminary exploratory data analysis can be conducted to determine relevant statistics for summarizing the MMSE data. 

# Data Description

The MMSE data separated by family history, was pulled from kaggle.com Rabie El Kharoua (2024). The group of patients with a family history of Alzheimer's contains 542 unique observations, while the other contains 1607 observations. The variable MMSE has scores ranging from 0-30, with the lower scores indicating a higher level of cognitive impairment. 

### Summary Statistics for Two Samples

The table below provides initial summary statistics that are helpful in gaining a stronger understanding of the MMSE scores for both groups. Note the values provided have been rounded to two decimal places. 

| Family History Alzheimers | Mean | Median | Standard Deviation | Minimum | Maximum | First Quartile | Third Quartile | n |
| ----------- | ----------- | -----|--------|--------------------|---------|---------|----------------|----------------|---|
| No    |  14.68   | 14.20 | 8.59 | 0.01 | 29.99 | 7.13 | 22.09 | 1607 |
| Yes   |  14.99  | 15.29 | 8.70 | 0.05 | 29.97 | 7.26 | 22.60 | 542 | 


### Check for Normality

Normal probability plots and histograms can show whether each group follows a normal distribution. Both visualizations showed that the data provided is not normally distributed, and seems to follow a uniform distribution. 

ADD PLOTS


<div style="display: flex;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/yes_hist.png" alt="Image 1" style="width: 45%; margin-right: 10px;" />
    <figcaption>Distribution of MMSE Scores for Patients Without a Family History of Alzheimer's</figcaption>
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/no_hist.png" alt="Image 2" style="width: 45%;" />
  <figcaption>Distribution of MMSE Scores for Patients Without a Family History of Alzheimer's</figcaption>
</div>


The null hypothesis for a Shapiro-Wilk test is that the data is normally distributed. With a p value lower than 0.05, we rejected the null hypothesis and concluded that the MMSE scores for both patient groups were not normally distributed.

ADD WILK TEST

### Check for Equal Variance

An F test can be administered to check for equal variance between two groups.The ratios of the variances was close to 1 (\~1.0268) so it could be concluded that the variances were equal.

ADD PLOT

### EDA Conclusions

The EDA conducted in this section gives valuable information about the distributions of our two samples that allows me to determine next steps for obtaining meaningful results. I learned that our MMSE data within both groups is not normally distributed. This means that I am not able to use parametric statistical methods to derive results, because one of the assumptions the test requires is for the data distribution to be approximately normal. therefore, I will need to use a different type of statistical method that does not require this assumption to be met. 

# Mann-Whitney Test

To continue this analysis,  a nonparametric statistical method holding the assumption that the distributions have similar shapes can be used. Since the normality assumption is not relevant for this test, we are able to continue with our original data to derive meaningful results. The Mann-Whitney test compares the ranks of the MMSE scores between groups with and without a family history of Alzheimer's disease. The null hypothesis for this test is that there is no difference in the distribution of MMSE scores for both groups, and the alternative is that there is a significant difference. 

ADD MAN WHIT

With a p value of 0.4949, we failed to reject the null hypothesis and concluded that there is not a significant difference between the distribution of MMSE scores in the two groups.

# Conclusions & Recommendations

After taking a closer look during the EDA portion of this report, I noticed my data was not normally distributed. I came to this conclusion after separating my data and examining Q-Q Plots, Shapiro-Wilk normality tests and distribution histograms - all of which failed to show a normal distribution. With this information, I decided to use the Mann-Whitney test which led me to fail to reject the hypothesis that there is no significant difference between patients with and without family history of Alzheimer's MMSE scores. 

In this analysis, pursuing a nonparametric method was the best option because of the nature of my data. However, in times where data is already normally distributed or can be easily transformed, it could be wiser to use parametric processes to make conclusions as there may be higher power. Despite this trade-off, I was still able to gain some valuable insight, and learn something new about my data.

# Recommendations for Future Studies

Although I was unable to conclude significant differences between my two groups, there may still be derivable significant differences within this dataset. I would encourage future research looking into how `Diagnosis` might be able to be predicted from other features recorded in the dataset. I would also encourage further EDA or additional research to determine important features as there is so much to learn about Alzheimer's disease. 
