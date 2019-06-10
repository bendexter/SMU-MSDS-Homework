---
title: "Week 6"
author: "Ben Tanaka"
date: "6/7/2019"
output: 
  html_document:
    keep_md: true
---



# *Question 1:*

```r
df <- read.csv(file="yob2016.txt", header=FALSE, sep=";")
setnames(df,old=c("V1","V2","V3"),new=c("First_Name","Gender","Total_Count"))
str(df)
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ First_Name : Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ Gender     : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Total_Count: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

```r
summary(df)
```

```
##    First_Name    Gender     Total_Count     
##  Aalijah:    2   F:18758   Min.   :    5.0  
##  Aaliyan:    2   M:14111   1st Qu.:    7.0  
##  Aamari :    2             Median :   12.0  
##  Aarian :    2             Mean   :  110.7  
##  Aarin  :    2             3rd Qu.:   30.0  
##  Aaris  :    2             Max.   :19414.0  
##  (Other):32857
```

```r
grep("yyy$",df$First_Name, value=TRUE)
```

```
## [1] "Fionayyy"
```

```r
y2016 <- df[!grepl("yyy$",df$First_Name),]
```

# *Question 2:*

```r
df <- read.csv(file="yob2015.txt", header=FALSE, sep=",")
setnames(df,old=c("V1","V2","V3"),new=c("First_Name","Gender","Total_Count"))
str(df)
```

```
## 'data.frame':	33063 obs. of  3 variables:
##  $ First_Name : Factor w/ 30553 levels "Aaban","Aabha",..: 9474 22858 26680 3771 12170 20927 344 9453 6252 11404 ...
##  $ Gender     : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Total_Count: int  20415 19638 17381 16340 15574 14871 12371 11766 11381 10283 ...
```

```r
summary(df)
```

```
##    First_Name    Gender     Total_Count     
##  Aalijah:    2   F:19054   Min.   :    5.0  
##  Aaliyah:    2   M:14009   1st Qu.:    7.0  
##  Aamari :    2             Median :   11.0  
##  Aarian :    2             Mean   :  111.4  
##  Aarion :    2             3rd Qu.:   30.0  
##  Aaron  :    2             Max.   :20415.0  
##  (Other):33051
```

```r
grep("yyy$",df$First_Name, value=TRUE)
```

```
## character(0)
```

```r
y2015 <- df[!grepl("yyy$",df$First_Name),]

tail(y2015,10)
```

```
##       First_Name Gender Total_Count
## 33054       Ziyu      M           5
## 33055       Zoel      M           5
## 33056      Zohar      M           5
## 33057     Zolton      M           5
## 33058       Zyah      M           5
## 33059     Zykell      M           5
## 33060     Zyking      M           5
## 33061      Zykir      M           5
## 33062      Zyrus      M           5
## 33063       Zyus      M           5
```

### Question 2: last 10 rows Observation

* The last two rows seem alsmot the same.  Zyrus and Zyus.  Could be a misspelling however, these are names so I have not clue if this is a data error or not.  And they all have a count of 5


```r
final <- merge(y2016,y2015, by=c("First_Name","Gender"))

setnames(final,old=c("Total_Count.x","Total_Count.y"),new=c("2016_Total_Cnts","2015_Total_Cnts"))
```

### Question 3:


```r
final$Total <- (final$`2016_Total_Cnts`+ final$`2015_Total_Cnts`)
final <- final[order(-final$Total),]
head(final,10)
```

```
##       First_Name Gender 2016_Total_Cnts 2015_Total_Cnts Total
## 8290        Emma      F           19414           20415 39829
## 19886     Olivia      F           19246           19638 38884
## 19594       Noah      M           19015           19594 38609
## 16114       Liam      M           18138           18330 36468
## 23273     Sophia      F           16070           17381 33451
## 3252         Ava      F           16237           16340 32577
## 17715      Mason      M           15192           16591 31783
## 25241    William      M           15668           15863 31531
## 10993      Jacob      M           14416           15914 30330
## 10682   Isabella      F           14722           15574 30296
```

```r
girls <- final %>% filter(final$Gender!="M") %>% slice(1:10)
View(girls)
girls %>% select(First_Name)
```

```
##    First_Name
## 1        Emma
## 2      Olivia
## 3      Sophia
## 4         Ava
## 5    Isabella
## 6         Mia
## 7   Charlotte
## 8     Abigail
## 9       Emily
## 10     Harper
```

```r
write.csv(girls,file="girl_names_top_ten.csv")
```
