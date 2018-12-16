---
published: true
category: blog
layout: post
title: Inspecting HOTS statistics with machine learning (R version)
---

# Gathering data

I download the data from [HOTSlogs](https://www.hotslogs.com/Info/API). Three tables are provided:


1. `HeroIDAndMapID.csv`: With the _name_ and _id_ of each hero and map.

```r
head(heroes_maps)
```
```
  ID       Name      Group SubGroup
1  0    Unknown                    
2  1    Abathur Specialist  Utility
3  2  Anub'arak    Warrior     Tank
4  3     Arthas    Warrior  Bruiser
5  4    Azmodan Specialist    Siege
6  5 Brightwing    Support   Healer
```

2. `Replays.csv`: With the _id_ of each replay indicating the type and time of the game and linking it to the map.

```r
head(replays, n=3)
```
```
   ReplayID GameMode.3.Quick.Match.4.Hero.League.5.Team.League.6.Unranked.Draft. MapID Replay.Length
1 157377095                                                                    3  1004      00:15:55
2 157386386                                                                    3  1005      00:15:32
3 160346787                                                                    3  1016      00:18:33
       Timestamp..UTC.
1 9/28/2018 7:36:46 PM
2 9/28/2018 7:36:49 PM
3 9/28/2018 7:36:49 PM
```

3. `ReplayCharacters.csv`: With the characteristics of each hero for each replay.

```r
head(replay_characters, n=3)
```
```
   ReplayID Is.Auto.Select HeroID Hero.Level Is.Winner MMR.Before In.Game.Level Takedowns Killing.Blows
1 157377095              0     37         20         0       2044            15         4             0
2 157377095              0     75          5         0       1974            15         4             0
3 157377095              0      7         20         0       2308            15         6             2
  Assists Deaths Highest.Kill.Streak Hero.Damage Siege.Damage Healing Self.Healing Damage.Taken
1       4      4                   3       15220        32767   33471            0        51545
2       4      3                   2       37355        43551      NA            0        17087
3       4     10                   2       37636        21441      NA        14753        91728
  Experience.Contribution Time.Spent.Dead Merc.Camp.Captures
1                    5853        00:01:58                  4
2                    6948        00:01:13                  0
3                    6126        00:03:50                  0
```

# Exploring data

## Table for heroes

The table `heroes_maps` contains the ID and name for both available heroes and for playable maps. We split this table in two, starting for heroes.


Heroes are classified in groups and subgroups. Which are these groups and subgroups?

```r
heroes <- heroes_maps[1:85, ]
as.character(unique(heroes$Group))
```
```
 [1] ""           "Specialist" "Warrior"    "Support"    "Assassin"  
```

```r
as.character(unique(heroes$SubGroup))
```
```
 [1] ""                 "Utility"          "Tank"             "Bruiser"         
 [5] "Siege"            "Healer"           "Sustained Damage" "Burst Damage"    
 [9] "Ambusher"         "Support" 
```

## Filtering replays to get 'quick match'

I will focus on the type of game '_quick match_'. To this we will filter the table for replays to get the replays for the type '_quick match_'.

```{r}
table(replays[ , 2])
```
```
     3      4      5      6 
874802 151781 251834 113522 
```

# Describing heroes statistics

* Variables that are categorical: `Is.Auto.Select`, `HeroID`, `Is.Winner`
* Variables that are continuous: `Hero.Level`, `MMR.Before`, `In.Game.Level`, `Takedowns`, `Killing.Blows`, `Assists`, `Deaths`, `Highest.Kill.Streak`, `Hero.Damage`, `Siege.Damage`, `Healing`, `Self.Healing`, `Damage.Taken`, `Experience.Contribution`, `Time.Spent.Dead`, `Merc.Camp.Captures`

From those, three variables needs attention: `MMR.Before`, `Healing`, and `Time.Spent.Dead`.


First, checking the results from `summary` on `MMR.Before` indicates that the 0.17% of the values are missing: 

```r
summary(quick_match_characters$MMR.Before)
```
```
 Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
 -105    1971    2200    2184    2413    4672   14450
```

These values can be imputed with the _mean_ of the `MMR.Before` of the other players (mean=`2184.402`).

Second, the `summary` on `Healing` shows that the 70.97% if the values are missing:

```r
summary(quick_match_characters$Healing)
```
```
 Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   1   14685   41039   44989   67098  392383 6208040 
```

For safety, we should discard this variable.

Finally, the variable `Time.Spent.Dead` is given as a time. To use include it in the analysis is better to convert it to seconds.

```r
head(quick_match_characters$Time.Spent.Dead, n=10)
```
```
 [1] "00:01:58" "00:01:13" "00:03:50" "00:00:29" "00:00:21" "00:03:58" "00:03:13"
 [8] "00:00:00" "00:00:41" "00:01:30"
```

## Remove unclassified heroes

There are heroes that were not properly classified. These are removed:
```r
sum(quick_match_characters$HeroName == "Unknown")
sum(quick_match_characters$HeroGroup == "")
sum(quick_match_characters$HeroSubGroup == "")
```

## Subset table for analysis

Then, I selected the top representative of each `HeroGroup`:

```r
data.frame(table(
  quick_match_characters$HeroName,
  quick_match_characters$HeroGroup
)) %>%
  group_by(Var1) %>%
  filter(Freq == max(Freq)) %>%
  arrange(Var1, Var2, Freq) %>%
  arrange(desc(Freq))
```
```
Johanna	Warrior	221957		
Murky	Specialist	220417		
Cassia	Assassin	217729		
Whitemane	Support	216647	
```

```r
quick_match_hero_group <- quick_match_characters %>%
  subset(HeroName == "Johanna" | HeroName == "Murky" |
           HeroName == "Cassia" | HeroName == "Whitemane") %>%
  group_by(HeroGroup) %>%
   do(head(., n=1000))
```

#  Analysis

In this first exploratory analysis I used to approaches:

1. Principal Component Analysis (PCA) to cluster the heroes.
2. Ensemble learning to find the best method to predict the hero based in their characteristics.

## Principal Component Analysis

I used `prcomp` to apply a principal components analysis by a singular value decomposition of the data matrix , centered and scaled.

```r
pca <- prcomp(t(dta[ , colums_for_prediction]),
  scale=TRUE, center=TRUE)
```

Component one and component two shows an arch, that it may be [taken into account](http://phylonetworks.blogspot.com/2012/12/distortions-and-artifacts-in-pca.html). In general, no principal component is able differentiate the heroes. The best ones are PC2, that seems to differentiate the red groups from the others, and PC4, that slightly differentiates the blue group from the others.

![PCA on 4000 HOTS hero characteristics]({{baseurl}}/assets/hots_pca_001.png)

## Ensemble learning

I created a list of 80% of the original data-set for training and 20% of the data for validation. Each algorithm using 10-fold cross validation.

### Linear regression analysis

```{r}
set.seed(7) 
fit.lda <- train(HeroGroup~., data=training, method="lda", 
  metric=metric, trControl=control)
```

### Logistic regression analysis

```{r, message=FALSE, warning=FALSE}
set.seed(7) 
fit.glm <- train(HeroGroup~., data=training, method="multinom",
  metric=metric, trControl=control)
```

### kNN analysis

```{r}
set.seed(7)
fit.knn <- train(HeroGroup~., data=training, method="knn", 
  metric=metric, trControl=control) 
```

### Random Forest 

```{r, eval=FALSE}
set.seed(7) 
fit.rf <- train(HeroGroup~., data=training, method="rf", 
  metric=metric, trControl=control) 
```

### Summarize accuracy of models 

![Accuracy of ensemble learning on 4000 HOTS hero characteristics]({{baseurl}}/assets/hots_accuracy_001.png)


```{r, eval=FALSE}
results <- resamples(list(lda=fit.lda, glm=fit.glm, knn=fit.knn, rf=fit.rf))
summary(results) 
```

In terms of accuracy, knn was the ones with the lowest performance (range: 0.57-0.64) and logistic-glm (_Penalized Multinomial Regression_) with the highest (0.73-0.79).

Then, I applied the logistic-glm on the validating data-set to obtain the confusion matrix:

```
Confusion Matrix and Statistics

            Reference
Prediction   Assassin Specialist Support Warrior
  Assassin        185          7       3       8
  Specialist        6        124      16      34
  Support           4         13     176      10
  Warrior           5         56       5     148

Overall Statistics
                                          
               Accuracy : 0.7912          
                 95% CI : (0.7614, 0.8189)
    No Information Rate : 0.25            
    P-Value [Acc > NIR] : <2e-16          
                                          
                  Kappa : 0.7217          
 Mcnemar's Test P-Value : 0.2192 
```