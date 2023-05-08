---
title: "RWE data"
author: "Luciana Burdman"
date: "2023-05-08"
output: html_document
---

HEOR project involving Real-World Evidence (RWE) and simulated data. This project aims to explore the impact of a new drug on patients' quality of life and healthcare resource utilization.

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(tidyverse)
library(ggplot2)
library(ggridges)
library(scales)
set.seed(123)
```

## Introduction
The purpose of this study is to evaluate the impact of a new drug on patient quality of life and healthcare resource utilization. The analysis will be based on simulated Real-World Evidence (RWE) data. We will generate data using the Monte Carlo simulation method, which will simulate the effects of the new drug on patients' quality of life and healthcare resource utilization. We will then use this simulated data to evaluate the efficacy of the drug.

## Simulating the Data
We will simulate data for two groups: a treatment group that receives the new drug and a control group that receives standard care. We will assume that patients in the treatment group experience a 10% improvement in their quality of life, as measured by a standardized quality of life scale. We will also assume that patients in the treatment group experience a 20% reduction in healthcare resource utilization, as measured by the number of hospitalizations and doctor visits. We will simulate data for 500 patients in each group.

```{r}
# Simulating data for the treatment group
quality_of_life_treatment <- rnorm(500, mean = 50, sd = 10)
quality_of_life_treatment <- quality_of_life_treatment * 1.1 # 10% improvement

healthcare_utilization_treatment <- rnorm(500, mean = 10, sd = 5)
healthcare_utilization_treatment <- healthcare_utilization_treatment * 0.8 # 20% reduction

treatment_df <- data.frame(quality_of_life = quality_of_life_treatment,
                           healthcare_utilization = healthcare_utilization_treatment,
                           group = "treatment")

# Simulating data for the control group
quality_of_life_control <- rnorm(500, mean = 50, sd = 10)
healthcare_utilization_control <- rnorm(500, mean = 10, sd = 5)

control_df <- data.frame(quality_of_life = quality_of_life_control,
                         healthcare_utilization = healthcare_utilization_control,
                         group = "control")

# Combining the treatment and control data
simulated_data <- bind_rows(treatment_df, control_df)

```

## Results
We will first compare the quality of life between the treatment and control groups using a density plot.

```{r}
ggplot(simulated_data, aes(x = quality_of_life, fill = group)) +
  geom_density(alpha = 0.5) +
  scale_fill_manual(values = c("#619CFF", "#F8766D")) +
  labs(title = "Quality of Life by Treatment Group",
       x = "Quality of Life",
       y = "Density") +
  theme_minimal()
```

<img src="https://github.com/lucianaburdman/RWE/blob/f8f4024db3fa8a1e589d59f13b5393c15e7b6633/Fig1.png">
Figure 1: Quality of Life by Treatment Group.

The density plot shows that the treatment group has a higher quality of life than the control group.

Next, we will compare the healthcare resource utilization between the treatment and control groups using a ridgeline plot.

```{r}
ggplot(simulated_data, aes(x = healthcare_utilization, y = group, fill = group)) +
  geom_density_ridges(alpha = 0.5, scale = 0.8) +
  scale_fill_manual(values = c("#619CFF", "#F8766D")) +
  labs(title = "Healthcare Resource Utilization by Treatment Group",
       x = "Healthcare Resource Utilization",
       y = "Group") +
  theme_minimal()
```


<img src="https://github.com/lucianaburdman/RWE/blob/f8f4024db3fa8a1e589d59f13b5393c15e7b6633/Fig2.png">
Figure 2: Healthcare Resource Utilization by Treatment Group

The ridgeline plot shows that the treatment group has a lower healthcare resource utilization than the control group.

## Statistical Test
To confirm whether the observed differences in quality of life and healthcare resource utilization between the treatment and control groups are statistically significant, we will perform a t-test for independent samples.

```{r}

# Performing t-test for quality of life
quality_of_life_ttest <- t.test(quality_of_life_treatment, quality_of_life_control, var.equal = TRUE)
quality_of_life_ttest

# Performing t-test for healthcare resource utilization
healthcare_utilization_ttest <- t.test(healthcare_utilization_treatment, healthcare_utilization_control, var.equal = TRUE)
healthcare_utilization_ttest

	Two Sample t-test

data:  quality_of_life_treatment and quality_of_life_control
t = 7.8408, df = 998, p-value = 1.145e-14
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 3.837468 6.399507
sample estimates:
mean of x mean of y 
 55.38049  50.26201 


	Two Sample t-test

data:  healthcare_utilization_treatment and healthcare_utilization_control
t = -7.8791, df = 998, p-value = 8.588e-15
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -2.876564 -1.729412
sample estimates:
mean of x mean of y 
 7.990661 10.293649 
```
The t-test results show that the differences in quality of life and healthcare resource utilization between the treatment and control groups are statistically significant (p < 0.05).

## Results Visualizations
To further visualize the differences between the treatment and control groups, we can create box plots for each variable.

```{r}
ggplot(simulated_data, aes(x = group, y = quality_of_life, fill = group)) +
  geom_boxplot() +
  scale_fill_manual(values = c("#619CFF", "#F8766D")) +
  labs(title = "Quality of Life by Treatment Group",
       x = "Group",
       y = "Quality of Life") +
  theme_minimal()

ggplot(simulated_data, aes(x = group, y = healthcare_utilization, fill = group)) +
  geom_boxplot() +
  scale_fill_manual(values = c("#619CFF", "#F8766D")) +
  labs(title = "Healthcare Resource Utilization by Treatment Group",
       x = "Group",
       y = "Healthcare Resource Utilization") +
  theme_minimal()

```

FIGURES


The box plots confirm the results of the density and ridgeline plots and show that the treatment group has a higher quality of life and lower healthcare resource utilization than the control group.

## Conclusion
The results of this study suggest that the new drug may have a positive impact on patient quality of life and healthcare resource utilization. Moreover, the t-test and box plots provide further evidence that the new drug has a significant impact on patient quality of life and healthcare resource utilization. Real-World Evidence data allowed us to evaluate the efficacy of the drug without conducting a costly and time-consuming randomized controlled trial. This results can inform healthcare policy and practice, and can aid in the decision-making process for drug approval and reimbursement.
