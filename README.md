# Spotify_Data_Analysis_Using_R

Understand and interpreting data, analyze the results using statistical techniques and provide appropriate visualizations using ggplot2.

This project has been developed to find the patterns

# The dataset for this project has been taken from the URL link:

https://www.kaggle.com/code/lusfernandotorres/spotify-top-hits-2000-2019-eda/data

# Data Cleaning

**Replacing Na values with median:**
```
> setwd("C:/R/5250_R_Project_Script") 
> spotify_songs<-read.csv("spotify_top_hits.csv") 
> View(spotify_songs) 
> dim(spotify_songs) [1] 2000 18 
> head(spotify_songs)  
> colSums(is.na(spotify_songs)) 
> spotify_songs[is.na(spotify_songs)]<-median(spotify_songs$popularity,na.rm=TRUE)
> colSums(is.na(spotify_songs))
```
**Deleting duplicate records:**
```
> View(spotify_songs)  
> dim(spotify_songs) 
[1] 2000 18 
> spotify_songs<-unique(spotify_songs) 
> dim(spotify_songs) 
[1] 1941 18 
> View(spotify_songs)
```
**Data type conversion from milliseconds to minutes:**
```
> spotify_songs <- spotify_songs %>% mutate(duration_min = (duration_ms / 1000)/ 60, year)
> View(spotify_songs)
```
**Removing irrelevant columns:**

> View(spotify_songs) 
> View(spotify_songs) > 
> dim(spotify_songs) 
[1] 1941 19 
> spotify_songs<-spotify_songs[,-c(3,9,11,14)] 
> View(spotify_songs) 
> dim(spotify_songs) 
[1] 1941 15

## Analysis and Visualizations

