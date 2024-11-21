---
layout: post
title:  "Nonparametric Tests on Alzheimer's Disease Data"
date: 2024-10-20
description: The data used in this paper looks at numerous medical and lifestyle records
  for 2,149 unique patients with and without diagnosed Alzheimer's disease. This analysis will first determine the distribution of Mini-Mental State Exam data for patients with and without a family history of Alzheimer's. Afterwards, a Mann-Whitney test will show that there is no significant difference between the two groups. Finally, conclusions and future study recommendations will be made based on the results of this paper's study. 
image: "/assets/img/435_report1/brain.png"
display_image: false  # change this to true to display the image below the banner 
---

# Introduction

Alzheimer's disease is a neurodegenerative disorder that affects people around the world, leading to memory loss and a severe decline in cognitive function. It is the most common cause of dementia as it accounts for 60-80% of cases. 

A widely used tool for assessing the cognitive function of Alzheimer's patients is the Mini-Mental State Exam (MMSE). This 30-point questionnaire evaluates a patient's cognitive functions such as orientation, attention, and memory. Lower scores on this exam indicate more severe impairment, but this alone will not entirely explain whether or not a person has Alzheimer's disease. Typically, it is used in combination with other clinical assessments and analyses to make a comprehensive diagnosis over time. However, examining MMSE scores can help medical professionals track and create healthcare plans accordingly. 

### Scientific Question

This paper aims to investigate whether there is a significant difference in cognitive function (measured by MMSE scores) between patients with and without a family history of Alzheimer's disease.

### Statistical Hypotheses

The hypotheses for this paper test if there is a significant difference in cognitive function score via the MMSE between our two groups. The null and alternative hypotheses are as follows, with a chosen alpha level of 0.05.

Null: $H_0:\mu_{Family History} = \mu_{NoFamilyHistory}$

Alternative: $ H_a: \mu_{Family History}\ne \mu_{NoFamilyHistory} $


### Procedure Used/Methodology

This procedure starts by grouping participants by their status: having or not having a family history of Alzheimer's disease. Some preliminary exploratory data analysis can be conducted to determine relevant statistics for summarizing the MMSE data. 

# Data Description

The MMSE data separated by family history, was pulled from [kaggle.com Rabie El Kharoua (2024)](https://www.kaggle.com/datasets/rabieelkharoua/alzheimers-disease-dataset). The group of patients with a family history of Alzheimer's contains 542 unique observations, while the other contains 1607 observations. The variable MMSE has scores ranging from 0-30, with the lower scores indicating a higher level of cognitive impairment. 

### Summary Statistics for Two Samples

The table below provides initial summary statistics that are helpful in gaining a stronger understanding of the MMSE scores for both groups. Note the values provided have been rounded to two decimal places for interpretability purposes. 

| Family History Alzheimers | Mean | Median | Standard Deviation | Minimum | Maximum | First Quartile | Third Quartile | n |
| ----------- | ----------- | -----|--------|--------------------|---------|---------|----------------|----------------|---|
| No    |  14.68   | 14.20 | 8.59 | 0.01 | 29.99 | 7.13 | 22.09 | 1607 |
| Yes   |  14.99  | 15.29 | 8.70 | 0.05 | 29.97 | 7.26 | 22.60 | 542 | 


### Check for Normality

Normal probability plots and histograms can show whether each group follows a normal distribution. Both visualizations showed that the data provided is not normally distributed, and seems to follow a uniform distribution. 

<figure>
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/qqplots.png" alt="Description of image" style="width:300px;height:350px;">
  <figcaption>QQ Plots</figcaption>
</figure>

<div style="display: flex;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/yes_hist.png" alt="Image 1" style="width: 45%; margin-right: 10px;" />

  <img src="{{site.url}}/{{site.baseurl}}/assets/img/435_report1/no_hist.png" alt="Image 2" style="width: 45%;" />
 
</div>


The null hypothesis for a Shapiro-Wilk test is that the data is normally distributed. With a p value lower than 0.05, we rejected the null hypothesis and concluded that the MMSE scores for both patient groups were not normally distributed.

| Shapiro-Wilk normality test |
| -----------------------------|
| data:  MMSE Scores for Patients Without a Family History of Alzheimer's |
| ------------------------------|
| W = 0.95262 | p-value < 2.2e-16 |
| -----------------------------------| 
| Shapiro-Wilk normality test |
| --------------------------------- |
| data:  MMSE Scores for Patients With a Family History of Alzheimer's |
| ------------------------------|
| W = 0.95105 | p-value = 2.05e-12 |

### Check for Equal Variance

An F test can be administered to check for equal variance between two groups.The ratios of the variances was close to 1 (~1.0268) so it could be concluded that the variances were equal.

        F test to compare two variances

        data:  MMSE by status
        F = 1.0268, num df = 541, denom df = 1606, p-value = 0.6977
        alternative hypothesis: true ratio of variances is not equal to 1
        95 percent confidence interval:
        0.8966642 1.1814337
        sample estimates:
        ratio of variances: 
                1.02679 

### EDA Conclusions

The EDA conducted in this section give valuable information about the distributions of our two samples, allowing for next steps to be taken in order to obtain meaningful results. We learned that our MMSE data within both groups is not normally distributed. This means that we are unable to use parametric statistical methods to derive results, because one of the assumptions the test requires is for the data distribution to be approximately normal. Therefore, a different type of statistical method that does not include a normality assumption is required 

# Mann-Whitney Test

To continue this analysis,  a nonparametric statistical method holding the assumption that the distributions have similar shapes was used. Since the normality assumption is not relevant for this test, we were able to continue with the original data to derive meaningful results. The Mann-Whitney test compared the ranks of the MMSE scores between groups with and without a family history of Alzheimer's disease. The null hypothesis for this test was that there is no difference in the distribution of MMSE scores for both groups, and the alternative was that there is a significant difference. 

``` r
wilcox.test(MMSE ~ FamilyHistoryAlzheimers, data = alz_data, alternative = 'two.sided')
```

          Wilcoxon rank sum test with continuity correction
         data:  MMSE by FamilyHistoryAlzheimers
         W = 426971, p-value = 0.4949
         alternative hypothesis: true location shift is not equal to 04

With a p value of 0.4949, we failed to reject the null hypothesis and concluded that there is not a significant difference between the distribution of MMSE scores in the two groups.

# Permutations and Randomized Combinations

To continue my analysis on the Alzheimer's Disease data, permutations and combination methods help to assess the difference in Mini-Mental State Examination (MMSE) scores between participants with and without a family history of Alzheimer's. A key reason why these methods are included is that permutation tests are not bound to any assumptions on the distribution of the data, and as previously noted, the data used in this analysis is not normally distributed. The goal is to explore how unusual the observed statistic acquired from the Mann-Whitney test is by testing if it occurred via random chance.

Given that the sample size of my data is relatively large (`r n` participants) using the traditional permutations and combinations formulas would be ineffective in R, because the values derived would be extremely high (R classifies them as `Inf`). Due to this constraint, we drew samples to approximate the distribution of differences that could be expected under the null hypothesis.

$H_0: \mu_{Family History} = \mu_{NoFamilyHistory}$

There is no significant difference in MMSE scores between individuals with a family history of Alzheimer's and those without

$H_A: \mu_{Family History}\ne \mu_{NoFamilyHistory}$

There is a significant difference in MMSE scores between individuals with a family history of Alzheimer's and those without

### Explanation of Techniques

A permutation test calculates a test statistic multiple times after randomly rearranging the data points to either group. For this data, that means randomly assigning MMSE scores to either participants with a family history of Alzheimer's, or without a family history of Alzheimer's. This results in a distribution of possible outcomes that could have been observed if the groupings were due to chance. For this study, 1,000 permutations were simulated, calculating the difference in sample means each time to test if the difference was greater than the observed difference in our original dataset.

In this context, combinations describe the reshuffling of data groupings in a similar manner to the permutations test, but we are sampling without replacement. This method does not enumerate all possible outcomes, but still provides insight into whether the observed differences in the original data are statistically significant.

Both the permutation and randomized combinations approaches support that our data follows what we would expect. The permutations and combinations density plots below illustrate the distributions of W statistics generated from 1,000 permutations and combinations. The original Mann-Whitney W statistic represented by the blue dashed line lies within 95% of both distributions, suggesting there is not a deviation from the expected outcome if the two family history groups were similar. 

# graphs

### Concluding Results

As recorded in the table above, both the permutation and combination tests produced p-values of `r results_perm$p_value` and `r results_comb$p_value`, which exceed the statistical significance threshold of 0.05. The confidence intervals for the W statistic include the null hypothesis value, further supporting the conclusion that the observed differences are likely due to random variation.

# Conclusions & Recommendations

After taking a closer look during the EDA portion of this report, the data did not appear to be normally distributed. By separating the data and examining Q-Q Plots, Shapiro-Wilk normality tests and distribution histograms, all failed to show a normal distribution. With this information, using the Mann-Whitney test made the most sense, and led to failing to reject the hypothesis that there was no significant difference between patients with and without family history of Alzheimer's MMSE scores. 

In this analysis, pursuing a nonparametric method was the best option because of the nature of the data. However, in times where data is already normally distributed or can be easily transformed, it may be wiser to use parametric processes to make conclusions as there may be higher power. Despite this trade-off, we were still able to gain some valuable insight, and learn something new about the data.

# Recommendations for Future Studies

Although this analysis was unable to find significant differences between the two groups, there may still be derivable significant differences within this dataset. I would encourage future research looking into how `Diagnosis` might be able to be predicted from other features recorded in the dataset. Further EDA or additional research to determine important features is also encouraged, as there is still so much to learn about Alzheimer's disease. 
