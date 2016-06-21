# Project
Repository for Case Study
---
title: "Exploratory Data Analysis and Summarization"
author: "Olufemi Adesanya"
date: "Monday, June 20, 2016"
output: 
   html_document:
    keep_md: true
    
---
##1.Introduction

####The scope of the work done in this project invovled the analysis of two datasets for exploratory data analysis.The two datasets were streameded from online into the R software.The datasets used for this analysis are  
####Gross Domestic Product data: This dataset include variables like countries, GDP, and ranking.
####Educational dataset iclude fields like countrycode, currencyunit, and systemoftrade. 

##2. Objective
#### The objective was to find out if there is a positive correlationbetween the GDP  and incomegroup variables groups.

##3.Functional Tasks
#### We imported our datasets from a site in the R software.Some of the functional tasks carried out include  data import from the website, data munging,data cleansing, and data analysis. In carrying out this analysis, transformation of the data was very important in order toget the correct figures as it relates to quantitative analysis results we got from analysing the datasets.Transformation like removing the "NAs", change of datatypes, formatting of values for uniformity were done. The two datasets were merged together using a common field present in these two dataset, the field name is called countrycode.




##### Answer 1.This code chunk shows how the data was imported into the R software from the site,tidied, and merged.Data was matched based on the country shortcode. The data was sorted in a descending order by GDP rank (so United States is last).  189 is the number of IDs that matched.
```{r}
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv" #import file
download.file(fileUrl, destfile="data.csv", method="curl")#download fileas a csv format
gdp1 <- read.csv("data.csv",na.strings=c(""))
fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv" #import file
download.file(fileUrl, destfile="data2.csv", method="curl")#download fileas a csv format
edu2 <- read.csv("data2.csv")
gdp1 <- gdp1[5:194,]#select rows
cleangdp1 <- subset(gdp1, select=c("X", "Gross.domestic.product.2012", "X.3")) #select variables to be used for analysis
cleanedu2 <- subset(edu2, select=c("CountryCode", "Income.Group")) #select variables to be used for analysis
library(plyr)
cleangdp1<- rename(cleangdp1, c(X= "countrycode", Gross.domestic.product.2012 ="Rank", X.3 = "GDP")) #Rename variables names
cleangdp1$GDP <-gsub(",","",cleangdp1$GDP)#replace commas
cleangdp1$GDP <-as.numeric(as.character(cleangdp1$GDP))# change variable datatype to numeric
cleangdp1$Rank <-as.numeric(as.character(cleangdp1$Rank))# change variable datatype to numeric
library(plyr)
cleanedu2 <- rename(cleanedu2, c(CountryCode="countrycode", Income.Group ="IncomeGroup"))#Rename variables names
mergedata <- merge(cleangdp1, cleanedu2, by.x="countrycode", by.y="countrycode")#merged the two clean datasets
```
#####Answer 2. The code chunk is to analyse the  13th country in the resulting data frame which is  KNA(St.Kitts and Nevis)
```{r}
library(plyr)
sort <- arrange(mergedata, desc(Rank))#sorting done in descending order using Rank variable
sort
```
#####Answer 3.This code was used to get the Average GDP rankings for the "High income:nonOECD" group, the average for this group is 91.91
```{r}
AvgData <-mergedata[mergedata$IncomeGroup=="High income: nonOECD",] #to get data with High income: nonOECD
AvgData
summary(AvgData) #to get the mean value
```
#####Answer 3.This code was used to get the Average GDP rankings for the "High income: OECD" group the average for this group is  32.97
```{r}
AvgData2 <-mergedata[mergedata$IncomeGroup=="High income: OECD",] #to get data with High income:OECD
AvgData2
summary(AvgData2) # to get the mean value
```

```{r}
library(ggplot2)
qplot(data=mergedata, x=countrycode, y=GDP, color=IncomeGroup) #scatterplot to plot GDP of all countries
```

#####Answer 5.This code was  used to cut the GDP ranking into 5 separate quantile groups. Make a table versus Income.Group. 
```{r}
sort_group <- arrange(mergedata, desc(GDP)) #sort data based on GDP variable descending
sort_group
group1 <- sort_group[1:38,] #select the first 38 rows based on the GDP variable 
group1 
```
#####Answer 5. Code used to analyse how many countries are Lower middle income but among the 38 nations with highest GDP - 5 countries
```{r}
group2<-group1[group1$IncomeGroup=="Lower middle income",]#to get countries that are lower middle income
group2
```
###Summary

###After a careful analysis of the data through the interpretation of the qplot, and analysis of the values in the tables, It was concluded that the there is a positive correlation between the GDP of countries and the Income Group.Majority of the High Income OECD countries have a higher GDP compared to countries in other groups,though there were few outliers, there is a linear relationship between the incomegroup and the GDP.
