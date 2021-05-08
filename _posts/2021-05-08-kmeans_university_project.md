---
layout: post
title: Simple project using KMeans + Spotify dataset
subtitle: Clustering and Predicting a music gender
cover-img: /assets/img/kmeans/kmeans_grouping_gender.png
thumbnail-img: /assets/img/kmeans/kmeans_grounping_second_gender.png
share-img: /assets/img/kmeans/kmeans_grouping_gender.png
tags: [C++, Computer Vision, Aruco]
readtime: true
---

# Problem Description

I had to do a small project using KMeans for a class of IA, I was asked to use KMeans to clustering and predicting whatever I wanted. Searching for datasets I found this one <a href="https://www.kaggle.com/yamaerenay/spotify-dataset-19212020-160k-tracks?select=data_by_genres.csv">one</a> that has artists and group bands data. I focused on six features:

- Instrumentalness: Proportion of the music containing instrumentals.
- Acousticness: Proportion of the music containing acousticness.
- Danceability: A metric that uses characteristics of music to tell what is the chance of the music being danceable.
- Energy: Measures the perceived intensity and activity of a song. More energy means that the music is more intense, dynamic, and loud.
- Liveness: Presence of an audience in a song.
- Speechiness: The presence of spoken words in the song.
- Genres: Music gender.

# Reshaping Data

First of all, of course, we need to look into our data and reshaping it. The table below shows the data before cleaning.
<br />

<div style="text-align:center;">
  <a href="/MyBlog/assets/img/kmeans/kmeans_table1.png">
    <img src="/MyBlog/assets/img/kmeans/kmeans_table1.png" alt="example">
  </a>
</div>
<br />

and the data after cleaning, going from 32.539 inputs to 13.982 inputs

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/kmeans/kmeans_table2.png">
    <img src="/MyBlog/assets/img/kmeans/kmeans_table2.png" alt="example">
  </a>
</div>
<br />

After this, I had to choose which genders I could use, So I looked at only data with more than 1000 inputs, I ended with three genders, Rap, Rock, and Classical Music. I tried to do some tests using Rock but there is no pattern at all in rock data since there are so many sub-genders. So I ended with Rap and Classical(It ended being good since we can see the difference between those genders).

You can see the code <a href="https://github.com/Cogitus/ia-project-2020.2/tree/main/Kmeans">here</a>, I used 80% of the data for training and 20% for validation, the data was normalized between 0 and 1, KMeans from scikit-learn was used to create two clusters.

### Results

The first result I'm going to show is the violin plot of all features for both clusters. We have some results that were already expected, such as the low Instrumentalnessand big Speechiness of Rap, and the opposite of it for Classical Music.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/kmeans/kmeans_grounping_gender.png">
    <img src="/MyBlog/assets/img/kmeans/kmeans_grounping_gender.png" alt="example">
  </a>
</div>
<br />

The next plot is a pair plot of features, evidencing that all features are distinguished, expected for liveness

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/kmeans/kmeans_grouping_second_gender.png">
    <img src="/MyBlog/assets/img/kmeans/kmeans_grouping_second_gender.png" alt="example">
  </a>
</div>
<br />

The last one is a 3d plot of three features, Danceability, Instrumentalnessand and Acousticness, blue is the cluster that "should" represent Classical music and yellow for Rap.

<br />
<div style="text-align:center;">
  <a href="/MyBlog/assets/img/kmeans/kmeans_grouping_3d_plot.png">
    <img src="/MyBlog/assets/img/kmeans/kmeans_grouping_3d_plot.png" alt="example">
  </a>
</div>
<br />

About the predicting, KMeans was able to predict the correct gender with an accuracy of 95,89%.

# Final Thoughts

IA is definitely not a thing for me, I rather code for Mobile than for IA or other areas we see in the course, but as a Computer Engineering we need to be able to solve problems, and I believe that now I have the maturity to understand that those class doesn't only teach us about the subject intend, but teach us to solve problems with the knowledge we have.
