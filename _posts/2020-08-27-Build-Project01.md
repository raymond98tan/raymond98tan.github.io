---
layout: post
title: Playlist organization by mood using Spotify data
subtitle: by Raymond Tan
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [project, music, spotify]
---

**Are we able to infer the mood of a song?**

In this blog, I outline the steps I conducted in order to answer this question through the use of data clustering.

### Retrieving my Spotify Data
The first step was to retrieve my spotify data. This was done through scraping the data off [http://organizeyourmusic.playlistmachinery.com/](http://organizeyourmusic.playlistmachinery.com/). This website provided data on a song's title, artist, top genre, year released, date added to the playlist, tempo,	energy,	dancibility, loudness, liveness, valence,	duration,	acoustics, speechiness,	and popularity. Because this website is only able to take 400 songs at a time, I created multiple playlists of 400 songs and individually loaded and scraped data from each playlist. The scraped data was input into an excel sheet which came out to 2963 songs.

The variables I used to detect mood were:

*   **nrgy**: Energy is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy.
*   **dnce**: Danceability describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength, and overall regularity. A value of 0.0 is least danceable and 1.0 is most danceable.
*   **dB**: The overall loudness of a track in decibels (dB). Loudness values are averaged across the entire track and are useful for comparing relative loudness of tracks. Loudness is the quality of a sound that is the primary psychological correlate of physical strength (amplitude). Values typical range between -60 and 0 db.
*   **live**: Detects the presence of an audience in the recording. Higher liveness values represent an increased probability that the track was performed live. A value above 0.8 provides strong likelihood that the track is live.
*   **val**: A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).
*   **acous**:  A confidence measure from 0.0 to 1.0 of whether the track is acoustic. 1.0 represents high confidence the track is acoustic.
*   **spch**: 	Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value. Values above 0.66 describe tracks that are probably made entirely of spoken words. Values between 0.33 and 0.66 describe tracks that may contain both music and speech, either in sections or layered, including such cases as rap music. Values below 0.33 most likely represent music and other non-speech-like tracks.

These definitions were taken from the [Spotipy API](https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-features/) which organizeyourmustic uses to gather song data.

### Normalizing the Data
Before inputing the data into an algorithm, I needed to make sure the data was normalized. All of the data was scaled, ranging from 1-100 in value except dB which ranged from -60 to 0. In order to scale the data I ran it through this function using the sklearn library:

![Normalization Function](/assets/img/normalization.JPG)

After scaling, every variable ranged from a value of 0.0-1.0.

### Clustering the songs
Now that the data is pre-processed, the next step is to cluster the songs and identify the mood within each cluster. I decided to use the K-Means Clustering Algorithm, which is an unsupervised learning algorithm which groups similar data points into a predefined number of groups to discover underlying patterns.

**How do we decide on the optimal number of clusters to split the data into?**
There are many ways to do this; one of methods is using an elbow plot. The elbow method runs k-means clustering on the dataset for a range of values for k and then for each value of k, computes an average score for all clusters. By default, the sum of square distances from each point to its assigned center is computed. Using this, we find a visual "elbow," in the plot, which is the optimal number of clusters to use.

This is the result putting our data into the elbow plot:

![Elbow Plot](/assets/img/elbowplot.JPG){: .mx-auto.d-block :}

From looking at the elbow plot, the elbow is not very obvious. However, we can see that the optimal number of clusters to use is 4 since after a k of 4, the sum of squared distances begin to level out.

### Visualizing the Clusters
With the number of clusters (k=4) decided, the next step was to explore the clusters formed by the K-means and identify the moods they may represent!
Since the dataset contains 7 features, we’re dealing with high dimensional data, which means it’s pretty hard to imagine, let alone plot.

Thankfully, we can use dimensionality reduction techniques to reduce the dimensions of our data, making it easier to visualise while retaining most of the information held within the data. I used a dimensionality reduction algorithm, Principal component analysis (PCA) to visualize the clusters.

![PCA plot of clusters](/assets/img/clusters.JPG){: .mx-auto.d-block :}

### Interpreting the Clusters
To better interpret the differences within the clusters, I created a bar plot of the means of each variable for each cluster:

![cluster_means](/assets/img/cluster_means.png){: .mx-auto.d-block :}
![cluster summaries](/assets/img/cluster_summaries.JPG){: .mx-auto.d-block :}

Looking at the graph, we see that clusters 0 and 2 both have low valence, or low positivity in the songs. They're very similar in their summary statistics, however, cluster 0 is slightly more energetic and slightly louder. The main difference between cluster 0 and cluster 2 though, is that cluster 0 has little acoustics, whereas cluster 2 has high acoustics.

In contrast, clusters 1 and 3 have high valences, or high positivity. However, cluster 3 has .24 more energy(.41 vs .65) and low acoustics(.15).

All of the clusters seem to have relatively similar levels of liveness and speechiness.

Taking a look at the songs within the clusters, 
