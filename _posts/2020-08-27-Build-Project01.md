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

![Normalization Function](/blob/master/_posts/normalization.JPG)

### Clustering the songs


### Visualizing the Clusters


### Interpreting the Clusters
