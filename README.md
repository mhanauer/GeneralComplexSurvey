# GeneralComplexSurvey
---
title: "Ch1"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

Here
```{r}
library(survey)
data(api)
apisrs$stype
srs_design = svydesign(id =~1,fpc=~fpc, data = apisrs)
svymean(~enroll, srs_design)
# Here we are using the survey weight pw which 30.7, because we do not have the total  
srsWeights = svydesign(id=~1, weights=~pw, data = apisrs)
svymean(~enroll, srsWeights)
test = apisrs$enroll
test = as.data.frame(test)
apply(test, 2, sum)
```
Here we seeing how to get total counts and means for categorical variables.
```{r}
svytotal (~stype, srs_design)
```
Stratified Sampling: Here we have a stratified independent sampling where we are taking random samples from the different strata.  I think the stratas were already created we are just calculating sample statistics based upon the strata.
```{r}
strat_design = svydesign(id=~1, strata=~stype, fpc = ~fpc, data = apistrat)
svymean(~enroll, strat_design)

```
Now we are working out replicate weights 
```{r}
boot_design = as.svrepdesign(strat_design, tyep = "bootstrap", replicates = 100)

svymean(~enroll, boot_design)
```
Here we are getting quatiles for the variable api00 
```{r}
apisrs$api00
svyquantile(~api00, strat_design, c(.025, .5, .75), ci = TRUE)
```
Here we are getting contigency tables
```{r}
head(apisrs)
tab = svymean(~interaction(ins, smoking, drop = TRUE), strat_design)
```
Here we are getting subpopulation estimates for the different strata
```{r}
emergy_high = subset(strat_design, emer>20)
svymean(~api00, emergy_high)

```
Chapter 3
```{r}
data(api)
library(survey)
clus2_design = svydesign(id = ~dnum+snum, fpc = ~ fpc1+fpc2, data = apiclus2)
svymean(~enroll, clus2_design)
```


