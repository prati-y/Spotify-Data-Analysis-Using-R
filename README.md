# Spotify_Data_Analysis_Using_R

In this project conducted in RStudio using the R programming language, we delved into Spotify data to unravel patterns and insights contributing to music's popularity. Employing statistical techniques and leveraging the power of ggplot2 for visualizations, our analysis aimed to provide a comprehensive understanding of what drives music preferences. The outcomes of this study offer valuable insights for Spotify's technical teams, enabling further exploration of trends and patterns in the music industry. The findings are instrumental in optimizing content distribution, supporting users more effectively, and ultimately enhancing profits while improving the overall user experience.

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
```
> View(spotify_songs) 
> View(spotify_songs) 
> dim(spotify_songs) 
[1] 1941 19 
> spotify_songs<-spotify_songs[,-c(3,9,11,14)] 
> View(spotify_songs) 
> dim(spotify_songs) 
[1] 1941 15
```
## Analysis and Visualizations

**What is the distribution of popularity by song? What are the top 10 songs?**
```
> library(ggplot2) 
> library(tidyverse) 
> ggplot(spotify_songs, aes( x = popularity)) +  
+ geom_histogram(bins=15) + 
+ labs(title="popularity distribution by song") + 
+ theme_minimal()
```

<img width="387" alt="image" src="https://user-images.githubusercontent.com/104661414/209245035-432227cf-926d-4971-8be9-eecc4120625f.png">

**How does the popularity change with the explicit content? Does explicit content positively, negatively, or neutral impact on popularity? **

```
> popularity_explicit_content<- spotify_songs %>% + 
+ ggplot(aes(x=popularity, fill=explicit))+ 
+ geom_histogram()+ 
+ ggtitle("Poplularity change with explicit content") 
> popularity_explicit_content
```
<img width="409" alt="image" src="https://user-images.githubusercontent.com/104661414/209245297-d2cf61e7-de85-4ebc-93ec-de6fe370b269.png">

**What is the proportion of high, medium, or low-rated songs? **

```
create popular_rating column for analysis’ 
> summary(spotify_songs$popularity) 
> spotify_songs$popularity 
> spotify_songs$popularity_rating <- as.factor( 
+   ifelse(spotify_songs$popularity<=60, "Low - Under 60", 
+          ifelse(spotify_songs$popularity<=80, "Med - 60-79", 
+                 ifelse(spotify_songs$popularity>80,'High - 80 above',"High - 80 above") 
+                 )) 
+ )
> spotify_songs$popularity_rating 

‘plot bar and pie graph'

> ggplot(spotify_songs, aes(x = popularity_rating))+ 
+ geom_bar() + 
+ labs(title="Popularity of songs rated High, Med, Low") + 
+ theme_minimal()

> ggplot(spotify_songs, aes(x=factor("“),fill=popularity_rating))+
+ geom_bar()+coord_polar(theta = “y”) +
+ scale_x_discrete(“") + 
+ labs(title= “Proportion of songs rated High, Med, Low”)
```
![image](https://user-images.githubusercontent.com/104661414/209245813-4b2aeb1d-cbca-4a74-abf1-1dc3ffeebfb4.png)

**Top 15 Artists with the most releases of the songs from the year 1998 – 2020?**

```
> Artist_Popular <- spotify_songs %>% count(artist, sort = TRUE, name = "Count") 
> Artist_Fil <- Artist_Popular %>% filter(Count >= 15) 
> par(mar = c(12, 5, 4, 2)+ 0.1)  
> barplot(Artist_Fil$Count, + 
+ ylab = "Number of songs", + 
+ col = "#80C4F5", + 
+ names.arg= Artist_Fil$artist, + 
+ width= 0.01, + 
+ ylim = c(0,20), +
+ las = 2 + )
```
![image](https://user-images.githubusercontent.com/104661414/209246004-158f91e5-5dfd-47b4-8750-ea8dddef2ab6.png)

**Has the length of songs changed through the years?**

```
> song_duration<- transmute(spotify_songs, duration_min = (duration_ms / 1000)/60 , year) 
> ggplot(song_duration, aes(x=year, y=duration_min)) + 
+ labs(title = "Duration of songs over years from 2000-2020") + 
+ labs(x="Year") + 
+ labs(y= "Duration in minutes") + 
+ geom_smooth() + 
+ geom_point()
```
![image](https://user-images.githubusercontent.com/104661414/209246101-ae5da804-014c-4458-a87d-0837313cecdd.png)

**How do the song's popularity and valency correlate with the duration?**
```
>ggplot(spotify_songs, aes(x=valence, y=popularity, color=duration_min)) + 
+ geom_point(size=2) + 
+ ggtitle("Relationship between song popularity and valency")
```
![image](https://user-images.githubusercontent.com/104661414/209246166-201ba244-d4db-4881-ace8-04b39addfbc6.png)

