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

## Conclusion
The results of this study suggest that the new drug may have a positive impact on patient quality of life and healthcare resource utilization. Real-World Evidence data allowed us to evaluate the efficacy of the drug without conducting a costly and time-consuming randomized controlled trial. Its results can inform healthcare policy and practice, and can aid in the decision-making process for drug approval and reimbursement.
