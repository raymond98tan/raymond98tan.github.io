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
*   **val**: A measure from 0.0 to 1.0 describing the musical positiveness conveyed by a track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry).
*   **acous**:  A confidence measure from 0.0 to 1.0 of whether the track is acoustic. 1.0 represents high confidence the track is acoustic.

These definitions were taken from the [Spotipy API](https://developer.spotify.com/documentation/web-api/reference/tracks/get-audio-features/) which organizeyourmustic uses to gather song data.

### Normalizing the Data
Before inputing the data into an algorithm, I needed to make sure the data was normalized. All of the data was scaled, ranging from 1-100 in value except dB which ranged from -60 to 0. In order to scale the data I ran it through this function using the sklearn library. After scaling, every variable ranged from a value of 0.0-1.0.

### Clustering the songs
Now that the data is pre-processed, the next step is to cluster the songs and identify the mood within each cluster. I decided to use the K-Means Clustering Algorithm, which is an unsupervised learning algorithm which groups similar data points into a predefined number of groups to discover underlying patterns.

**How do we decide on the optimal number of clusters to split the data into?**
There are many ways to do this; one of methods is using an elbow plot. The elbow method runs k-means clustering on the dataset for a range of values for k and then for each value of k, computes an average score for all clusters. By default, the sum of square distances from each point to its assigned center is computed. Using this, we find a visual "elbow," in the plot, which is the optimal number of clusters to use.

This is the result putting our data into the elbow plot:

![Elbow Plot](/assets/img/elbowplot.JPG){: .mx-auto.d-block :}

From looking at the elbow plot, the elbow is not very obvious. However, we can see that the optimal number of clusters to use is 4 since after a k of 4, the sum of squared distances begin to level out.

### Visualizing the Clusters
With the number of clusters (k=4) decided, the next step was to explore the clusters formed by the K-means and identify the moods they may represent!
Since the dataset contains 5 features, we’re dealing with high dimensional data, which means it’s pretty hard to imagine, let alone plot.

Thankfully, we can use dimensionality reduction techniques to reduce the dimensions of our data, making it easier to visualise while retaining most of the information held within the data. I used a dimensionality reduction algorithm, Principal component analysis (PCA) to visualize the clusters.

![PCA plot of clusters](/assets/img/clusters.JPG){: .mx-auto.d-block :}

### Interpreting the Clusters
To better interpret the differences within the clusters, I created a bar plot of the means of each variable for each cluster:

![cluster_means](/assets/img/cluster_means2.png){: .mx-auto.d-block :}
![cluster summaries](/assets/img/cluster_summaries.JPG){: .mx-auto.d-block :}

Looking at the graph, we see that clusters 0 and 3 both have low valences(.24 and .28 respectively), or low positivity in the songs. Cluster 0 has the highest acoustics at 0.72. Cluster 3 has .2 more energy and slightly higher dancability as well as loudness.

In contrast, clusters 1 and 2 have high valences(.65 and .61), or high positivity. Cluster 1 has the lowest acoustics but the highest levels for every other feature. Cluster 2 has high acoustics at 0.64.

The energy levels of the clusters go from .3, .4, .5, and .6(not in the respective order of the clustering), which raises suspicion that that may be an area of how they are clustered.

### Identifying the Moods
Now that we've interpreted some of the differences within the clusters, we're able to assign a mood to each cluster by looking at the songs within them and identifying the key emotions associated with the majority of songs in a particular cluster.

#### Cluster 0
1. Frank Ocean - Nikes (**Alt r&b**)
2. Daniel Caesar - Who Hurt You? (**Contemporary r&b**)
3. Phora - Faithful (**Deep Pop r&b**)
4. Khai - Do you go up (**Un-Genred**)
5. Pink Sweat$ - Honesty (**Alt r&b**)

![cluster0 means](/assets/img/cluster0_means.JPG)

Taking a look at the songs within the clusters, cluster 0's songs are a lot slower, less energetic, and has high acoustics. Some of the songs from my lofi playlist ended up in this cluster as well. Therefore, I'll identify this playlist cluster as "Chill."

#### Cluster 1
1. Biz Markie - Just a Friend (**Hip Hop**)
2. Mario - Let me love you (**Dance Pop**)
3. A Boogie Wit da Hoodie - Drowning (**Melodic Rap**)
4. Jay Park - Yacht (**K-pop**)
5. Doja Cat - Bottom Bitch (**Indie**)

![cluster1 means](/assets/img/cluster1_means.JPG)

Looking at cluster 1, the songs are a lot more high energy and very danceable. Therefore, I'll identify this playlist cluster as "Bump."

#### Cluster 2
1. Lucky Daye - Buying Time (**Alt r&b**)
2. BROCKHAMPTON - FACE (**Boy band**)
3. J. Cole - Love Yourz (**Conscious Hip Hop**)
4. Col3trane - Heroine (**Alt r&b**)
5. Russ - Summer at 7 (**Hawaiian Hip Hop**)

![cluster2 means](/assets/img/cluster2_means.JPG)

Looking at cluster 2, the songs are higher positivity, higher acoustics, and groovy(for a lack of a better word). These are the type of songs where I'd want to jam to at night because of the lower energy level without any negative vibes. Therefore, I'll identify this playlist cluster as "rnb and hip hop"

#### Cluster 3
1. SZA - Love Galore (**Pop**)
2. XXXTENTACION - Save Me (**Emo rap**)
3. Tyler, The Creator - EARFQUAKE (**Hip Hop**)
4. Yo Trane - Moonlight (**Alt r&b**)
5. keshi - like i need u (**Indie r&b**)

![cluster3 means](/assets/img/cluster3_means.JPG)

Within Cluster 3, there is a low valence level, however, it seems to be more of a mix of love songs with a more energetic, upbeat rhythm. Some of the songs within the cluser aren't about love at all though. The songs within the cluster are songs I'd listen to on a late night drive, therefore, I'll identify the playlist cluster as that: "late night drives."

### Conclusion
This project was a fun way to explore my Spotify playlist data and learn more about my musical taste. With musical evaluation, there's always the problem of human subjectivity to what emotions and feelings a song can create. Despite this, I think slowly begin to understand these feelings on a wider scale after understanding each unique listener’s taste. Using K-Means clustering, there were many songs in the clustered playlists that I would have put in their own playlists. For example, many lofi songs showed up in clusters 0 and 3, though I would have a separate playlist for them simply for it's genre. If done again, I may have created clusters for my individual playlists by the genre in order to see the clusters within that certain genre.
