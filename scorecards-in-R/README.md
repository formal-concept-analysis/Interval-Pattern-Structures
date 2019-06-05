**Building Scorecards in R**

In credit scoring banks aim to discriminate between creditworthy borrowers and potentially delinquent. 
From data science standpoint, they solve binary classification problem.
The most wide spread algorithm for this purpose is a scorecard, which is in effect a logistic regression run on transformed features.
The features are transformed according to weight-of-evidence transformation (WOE).

---

**Apply scorecards to Give Me Some Credit data**

```{r}
library(pROC)
setwd("GiveMeSomeCredit")
source("WOER.R")
```
**Read data**
```{r}
tr = read.table(file = "df_train.csv", sep = ",", header = T)
vl = read.table(file = "df_test.csv", sep = ",", header = T)
```

**Create scorecard**
```{r}
myCard = make.scorecard(d = tr, target = "SeriousDlqin2yrs", number.of.vars = 7)
```
**Score validation sample**
```{r}
vl_scored = do.score(data = vl, scorecard = myCard)
```
**Gini on validation sample**
```{r}
2*roc(predictor = vl_scored$score_norm, response = vl_scored$SeriousDlqin2yrs)$auc -1
```