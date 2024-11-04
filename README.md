# Exploratory-Data-Analysis-Spotify2023

## Introduction of the Project
 This project utilizes Python to allow for the very detailed, data-driven exploration of the 2023 dataset of Spotify, so we get a glimpse of the dynamics of modern music consumption. Through analysis and visualization of this data, we can find out what artists and genres dominate how content is organized and which patterns of engagement shape user experiences. This growing influence of Spotify in the realm of music streaming and its significance as a case study to understand the ways through which digital media develops and shifts listener behavior in an evolving industry is unparalleled.

### Analysis Objectives
* [Overview of Dataset](#overview-of-dataset)
* [Basic Descriptive Statistics](#basic-descriptive-statistics)
* [Top Performers](#top-performers)
* [Temporal Trends](#temporal-trends)
* [Genre and Music Characteristics](#genre-and-music-characteristics)
* [Platform Popularity](#platform-popularity)
* [Advanced Analysis](#advanced-analysis)

--- 




## Overview of Dataset

##### CODE 

```
# Import different type of liblaries

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

```

##### CODE 

```
# Load and display the full data from the CSV file
data = pd.read_csv('spotify-2023.csv', encoding='latin1')
data

```

#### 1. How many rows and columns does the dataset contain?

- Spotify 2023 data includes 953 rows with 24 columns.
This forms a different row for each entry, including song titles and other pertinent information.
The columns show each attribute depending on the entry.



#### 2.1 What are the data types of each column?


- The data set has some columns, for example, track_name and artist(s)_name are strings, while the artist_count, released_year, released_month, released_day are integers. And the in_spotify_playlists, in_spotify_charts, in_apple_playlists are also integers showing if a song exists in a playlist or not. Streams is an integer column with the count of the times a song has been streamed. Bpm is a float column representing tempo. The key and mode columns contain string representations of the musical key, and features like audio danceability_%, valence_%, energy_%, acousticness_%, instrumentalness_%, liveness_%, speechiness_% are captured as float.

#### 2.2 Are there any missing values?
![image](https://github.com/user-attachments/assets/b1fb1e03-37cf-47ad-b076-5a2e376cdba1)

![image](https://github.com/user-attachments/assets/c7d903ed-1eda-471b-8edc-4b24ce427264)


- A value of "0" can represent a feature in the analysis while missing values occur in columns that include "in_shazam_charts" and "key," .
##### CODE 
```



# Loop through each column to find any missing values
for column in data.columns:
    if data[column].isnull().any():  # If the column has missing values
        missing_data = data[data[column].isnull()]  # Get rows with missing values in the column
        print(f"Missing values in '{column}':")  # Display the column name with missing values
        print(missing_data[['track_name', column]])  # Print track names with missing data in the column
        print()  # Blank line for clarity between results



```
---






## Basic Descriptive Statistics

#### 1. What are the mean, median, and standard deviation of the streams column?

![image](https://github.com/user-attachments/assets/a57ace7f-84aa-4ffc-b976-a751b6ebfec5)


- The mean streams are 514,483,587.17, and the median is 290,833,204.00. This means that half of the songs have streams below this value. The standard deviation is 567,054,532.48, which is very high. The statistics represent the average performance and diversity in song popularity.
##### CODE 
```
# Convert the 'streams' column to numeric, coercing errors to NaN
data['streams'] = pd.to_numeric(data['streams'], errors='coerce')

# Calculate mean, median, and standard deviation of the 'streams' column
mean_streams = data['streams'].mean()  # Mean of streams
median_streams = data['streams'].median()  # Median of streams
std_streams = data['streams'].std()  # Standard deviation of streams

# Create a DataFrame to hold the statistical results
results = pd.DataFrame({
    'Statistic': ['Mean', 'Median', 'Standard Deviation'],  # Statistic labels
    'Value': [mean_streams, median_streams, std_streams]  # Corresponding values
})

# Print the results table with formatted statistics
print("Results:")  # Title for the results
print(f"Mean: {mean_streams:.2f}")  # Display mean
print(f"Median: {median_streams:.2f}")  # Display median
print(f"Standard Deviation: {std_streams:.2f}")  # Display standard deviation

```


#### 2. What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?

![image](https://github.com/user-attachments/assets/9cb40b18-eaae-4730-8678-2d08fc00e2e5)



- Variable released_year is right-skewed with a large spike at around 2020. There are many outliers of older releases, especially for those before 2000. The artist_count distribution is also right-skewed. Most releases have 1-2 artists, but there are very few with many artists. The box plots indicate this: recent years have the highest release frequency, while solo or duo releases dominate.
##### CODE 

```
# Create histograms for 'released_year' and 'artist_count'
plt.figure(figsize=(14, 6))  # Set figure size for the histograms

# Histogram for 'released_year'
plt.subplot(1, 2, 1)  # Create a subplot for the first histogram
sns.histplot(data['released_year'], bins=20, kde=True, color='skyblue')  # Plot histogram with kernel density estimate (KDE)
plt.title('Distribution of Released Year')  # Title for the histogram
plt.xlabel('Released Year')  # X-axis label
plt.ylabel('Frequency')  # Y-axis label

# Histogram for 'artist_count'
plt.subplot(1, 2, 2)  # Create a subplot for the second histogram
sns.histplot(data['artist_count'], bins=10, kde=True, color='lightgreen')  # Plot histogram with KDE
plt.title('Distribution of Artist Count')  # Title for the histogram
plt.xlabel('Artist Count')  # X-axis label
plt.ylabel('Frequency')  # Y-axis label

plt.tight_layout()  # Adjust layout for better spacing
plt.show()  # Display the histograms

# Create box plots for 'released_year' and 'artist_count'
plt.figure(figsize=(14, 6))  # Set figure size for the box plots

# Box plot for 'released_year'
plt.subplot(1, 2, 1)  # Create a subplot for the first box plot
sns.boxplot(x=data['released_year'], color='skyblue')  # Plot box plot for released year
plt.title('Box Plot of Released Year')  # Title for the box plot
plt.xlabel('Released Year')  # X-axis label

# Box plot for 'artist_count'
plt.subplot(1, 2, 2)  # Create a subplot for the second box plot
sns.boxplot(x=data['artist_count'], color='lightgreen')  # Plot box plot for artist count
plt.title('Box Plot of Artist Count')  # Title for the box plot
plt.xlabel('Artist Count')  # X-axis label

plt.tight_layout()  # Adjust layout for better spacing
plt.show()  # Display the box plots
```

---

## Top Performers
#### 1. Which track has the highest number of streams? Display the top 5 most streamed tracks.
![image](https://github.com/user-attachments/assets/c95927c5-4426-4d6d-a2bf-e682b1bcaaa6)



- Among different tracks, it has been witnessed that songs like "Blinding Lights" by The Weeknd are the top streamed to the tune of 3.7 billion streams in total, then comes Ed Sheeran with "Shape of You," which attracted 3.56 billion streams, while Lewis Capaldi's song "Someone You Loved" reached out to 2.89 billion streams in total followed by Tones and I's "Dance Monkey" with a total reach of 2.86 billion streams in total along with Post Malone and Swae Lee for "Sunflower" for 2.81 billion streams.
##### CODE 
```
# Find the top 5 most streamed tracks
top_streamed_tracks = data[['track_name', 'artist(s)_name', 'streams']].nlargest(5, 'streams')

# Display the results
print("Top 5 Most Streamed Tracks:")
print(top_streamed_tracks)

```

#### 2. Who are the top 5 most frequent artists based on the number of tracks in the dataset?
![image](https://github.com/user-attachments/assets/06e0f583-0899-4c92-8570-0975c7f8f53f)


- Bad Bunny leads with 40 tracks, followed by Taylor Swift with 38, then The Weeknd with 37, SZA at 23, and Kendrick Lamar at 23. These artists show some of the main trends in current music. Bad Bunny was driving Latin music to be a growing trend, Taylor Swift showed versatility with her genres, The Weeknd represented the fusion of R&B and pop, and SZA and Kendrick Lamar for R&B and hip-hop. 

##### CODE 
```


# Load the dataset
data = pd.read_csv('spotify-2023.csv', encoding='latin1')

# Count occurrences of each artist, including collaborations
top_artists = data['artist(s)_name'].str.split(',').explode().str.strip().value_counts().head(5)

# Display the results
print("Top 5 Most Frequent Artists (including collaborations):")
print(top_artists)

```
---


## Temporal Trends
#### 1. Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.

![image](https://github.com/user-attachments/assets/0a28a91a-30aa-4de0-8b2c-3488273dc251)


- Track releases increased gradually from the 1930s to the early 2000s, then accelerated from 2015 and peaked in 2021ᅳpresumably because of digital distribution. While releases declined slightly after 2021, they are still much higher than before 2015.
#### 2. Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?


![image](https://github.com/user-attachments/assets/0aca6ac5-5241-4edd-b92e-ee1cfed9df3d)


- Seasonally, January and May release the most, probably due to new year promotions and pre-summer launches, while August and September release the least. This information shows increasing annual production and seasonal patterns in music releases.
##### CODE 
```
# Extract the year and month from the 'released_year' column
data['released_year'] = data['released_year'].astype(str)  # Convert 'released_year' to string type
data['year'] = data['released_year'].str[:4]  # Extract the first four characters as the year
data['month'] = data['released_month']  # Assign 'released_month' column to new 'month' column

# Count the number of tracks released per year
tracks_per_year = data['year'].value_counts().sort_index()  # Count and sort track releases by year

# Plot the number of tracks released per year
plt.figure(figsize=(10, 5))  # Set figure size
plt.plot(tracks_per_year.index, tracks_per_year.values, marker='o', linestyle='-')  # Line plot
plt.title('Number of Tracks Released Per Year')  # Title of the plot
plt.xlabel('Year')  # X-axis label
plt.ylabel('Number of Tracks Released')  # Y-axis label
plt.grid()  # Add grid for better readability

# Adjust x-axis ticks for better visibility
plt.xticks(rotation=90)  # Rotate x-axis labels
plt.tight_layout()  # Prevent clipping of tick labels
plt.show()  # Display the plot

# Count the number of tracks released per month
tracks_per_month = data['month'].value_counts().sort_index()  # Count and sort track releases by month

# Plot the number of tracks released per month
plt.figure(figsize=(10, 5))  # Set figure size
plt.bar(tracks_per_month.index, tracks_per_month.values, color='skyblue')  # Bar plot
plt.title('Number of Tracks Released Per Month')  # Title of the plot
plt.xlabel('Month')  # X-axis label
plt.ylabel('Number of Tracks Released')  # Y-axis label
plt.xticks(tracks_per_month.index, ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 
                                      'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])  # Set month labels
plt.grid(axis='y')  # Add gridlines along the y-axis
plt.show()  # Display the plot

# Identify the month with the most releases
most_releases_month = tracks_per_month.idxmax()  # Get the month with the highest track count
most_releases_count = tracks_per_month.max()  # Get the count of tracks for that month

# Print the result
print(f"The month with the most releases is {most_releases_month} with {most_releases_count} tracks.")  # Display the month and count


```
---

## Genre and Music Characteristics
This section analyzes various genres and their characteristics.

#### 1. Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?

![image](https://github.com/user-attachments/assets/b1bb805a-a23e-4ae2-b12b-3af373cf651e)

- The Audio features BPM, danceability, energy, and valence individually are poor predictors for the success of a song on the streaming platforms. There are probably more influential factors than the intrinsic audio features, such as artist popularity, social trends, marketing, and playlist inclusion, in the success of streaming.
##### CODE 
```
# Load the dataset
data = pd.read_csv('spotify-2023.csv', encoding='latin1')  # Read the CSV file with specified encoding

# Select only the necessary columns
data_subset = data[['streams', 'bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%']]  # Create a subset of the DataFrame with relevant columns

# Drop rows with missing values
data_subset = data_subset.dropna()  # Remove any rows that contain missing values

# Create scatter plots with readability adjustments
plt.figure(figsize=(16, 12))  # Set the figure size for the plots

# Scatter plot for Streams vs. BPM
plt.subplot(2, 2, 1)  # Create a subplot in a 2x2 grid, first plot
plt.scatter(data_subset['bpm'], data_subset['streams'], color='blue', alpha=0.5)  # Scatter plot of BPM vs. streams
plt.title('Streams vs BPM', fontsize=14)  # Title of the plot
plt.xlabel('BPM (Beats Per Minute)', fontsize=12)  # X-axis label
plt.ylabel('Number of Streams', fontsize=12)  # Y-axis label
plt.yscale('log')  # Set Y-axis to log scale for better visibility of data distribution
plt.xticks(fontsize=10)  # Set font size for X-axis ticks
plt.yticks(fontsize=10)  # Set font size for Y-axis ticks
plt.grid(True)  # Enable grid for better readability

# Scatter plot for Streams vs Danceability
plt.subplot(2, 2, 2)  # Second plot in the grid
plt.scatter(data_subset['danceability_%'], data_subset['streams'], color='green', alpha=0.5)  # Scatter plot of danceability vs. streams
plt.title('Streams vs Danceability %', fontsize=14)  # Title of the plot
plt.xlabel('Danceability (%)', fontsize=12)  # X-axis label
plt.ylabel('Number of Streams', fontsize=12)  # Y-axis label
plt.yscale('log')  # Set Y-axis to log scale
plt.xticks(fontsize=10)  # Set font size for X-axis ticks
plt.yticks(fontsize=10)  # Set font size for Y-axis ticks
plt.grid(True)  # Enable grid

# Scatter plot for Streams vs Energy
plt.subplot(2, 2, 3)  # Third plot in the grid
plt.scatter(data_subset['energy_%'], data_subset['streams'], color='red', alpha=0.5)  # Scatter plot of energy vs. streams
plt.title('Streams vs Energy %', fontsize=14)  # Title of the plot
plt.xlabel('Energy (%)', fontsize=12)  # X-axis label
plt.ylabel('Number of Streams', fontsize=12)  # Y-axis label
plt.yscale('log')  # Set Y-axis to log scale
plt.xticks(fontsize=10)  # Set font size for X-axis ticks
plt.yticks(fontsize=10)  # Set font size for Y-axis ticks
plt.grid(True)  # Enable grid

# Scatter plot for Streams vs Valence
plt.subplot(2, 2, 4)  # Fourth plot in the grid
plt.scatter(data_subset['valence_%'], data_subset['streams'], color='purple', alpha=0.5)  # Scatter plot of valence vs. streams
plt.title('Streams vs Valence %', fontsize=14)  # Title of the plot
plt.xlabel('Valence (%)', fontsize=12)  # X-axis label
plt.ylabel('Number of Streams', fontsize=12)  # Y-axis label
plt.yscale('log')  # Set Y-axis to log scale
plt.xticks(fontsize=10)  # Set font size for X-axis ticks
plt.yticks(fontsize=10)  # Set font size for Y-axis ticks
plt.grid(True)  # Enable grid

# Adjust layout with added spacing
plt.tight_layout(pad=3.0)  # Adjust layout to prevent overlap
plt.show()  # Display the plots

# Calculate and print correlation coefficients
dance_energy_corr = data_subset['danceability_%'].corr(data_subset['energy_%'])  # Calculate correlation between danceability and energy
valence_acoustic_corr = data_subset['valence_%'].corr(data_subset['acousticness_%'])  # Calculate correlation between valence and acousticness

# Print the correlation results
print(f'Correlation between Danceability % and Energy %: {dance_energy_corr:.2f}')
print(f'Correlation between Valence % and Acousticness %: {valence_acoustic_corr:.2f}')


```


#### 2. Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

![image](https://github.com/user-attachments/assets/5bc1a002-f276-4c68-8e2b-176a9c93d55b)


- The analysis reveals a very weak positive correlation of 0.20 between danceability and energy, and the correlation between valence and acousticness is very weakly negative at -0.08. This infers that these attributes all contribute independently to a character of a song, providing for varied combinations in popular music.
##### CODE 
```


# Calculate correlation matrices for the pairs
danceability_energy_corr = data[['danceability_%', 'energy_%']].corr()
valence_acousticness_corr = data[['valence_%', 'acousticness_%']].corr()

# Display the correlation matrices
print("The correlation between danceability_% and energy_% is")
print(danceability_energy_corr)

print("\nThe correlation between valence_% and acousticness_% is")
print(valence_acousticness_corr)


```
---

## Platform Popularity

#### 1. How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

![image](https://github.com/user-attachments/assets/6e874e93-f5f4-4a18-9b50-2caddd349ea2)



- The dataset has quite some differences in track counts: the Spotify Playlists is a leader with 4,955,719 tracks and offer a huge amount of music. The Spotify Charts have 11,445 tracks targeting popular, trending songs; Apple Playlists feature 64,625 tracks that are curated, though less diverse, offering than the vast library of Spotify. The former offers playlists for a diverse taste, while the latter features the most recent hits.

##### CODE 
```
# Count the number of tracks in each playlist category
spotify_playlists_count = data['in_spotify_playlists'].sum()  # Total tracks in Spotify playlists
spotify_charts_count = data['in_spotify_charts'].sum()  # Total tracks in Spotify charts
apple_playlists_count = data['in_apple_playlists'].sum()  # Total tracks in Apple playlists

# Create lists for platforms and their corresponding counts
platforms = ['Spotify Playlists', 'Spotify Charts', 'Apple Playlists']  # Platform names
counts = [spotify_playlists_count, spotify_charts_count, apple_playlists_count]  # Track counts

# Create a simple bar chart
plt.figure(figsize=(8, 5))  # Set figure size
plt.bar(platforms, counts, color=['blue', 'orange', 'green'])  # Bar chart with specified colors

# Adding titles and labels
plt.title('Number of Tracks in Different Playlists')  # Chart title
plt.xlabel('Platform')  # X-axis label
plt.ylabel('Number of Tracks')  # Y-axis label

# Display the value on top of each bar
for i in range(len(counts)):  # Iterate through counts
    plt.text(i, counts[i], counts[i], ha='center', va='bottom')  # Display count above the bar

# Show the plot
plt.show()  # Render the plot

# Determine which platform has the most tracks
most_favored_platform = platforms[counts.index(max(counts))]  # Get platform with the highest track count
most_favored_count = max(counts)  # Get the count of tracks for that platform

# Print the result
print(f"The platform that favors the most popular tracks is {most_favored_platform} with {most_favored_count} tracks.")  # Output the result


````

## Advanced Analysis
#### 1. Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?

![image](https://github.com/user-attachments/assets/215d561a-ab15-4234-85f6-275d4a7ac14b)



- The musical key and mode ( Major vs. Minor) do not significantly affect the stream counts because no clear patterns were found in the data. Rather, it is the popularity of the artist, marketing, genre appeal, and cultural trends that seem to have a more significant effect on the success of a song on streaming platforms. Key and mode are important musically, but they do not have a significant effect on listener engagement, at least not as much as these broader factors do.

#### 2. Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

![image](https://github.com/user-attachments/assets/61d7c5f7-de7c-470a-8576-e7246e7b4022)


- Examples include those artists, Taylor Swift, The Weeknd, Ed Sheeran, Harry Styles, who are mostly found on the playlists and charts. Their horizontal bar charts indicate such artists are more important, therefore their visibilities increase, making music popular among their fans. Moreover, from the above results of this study, it is evident that main stream genres such as pop, R&B, hip-hop, etc. hold a higher probability of being noticed and, therefore more placements in the playlists and charts. It would thereby stress the role of the popularity of the artists involved and the appeal of a particular genre in ensuring successful music streaming.
##### CODE 
```
# Load the dataset
data = pd.read_csv('spotify-2023.csv', encoding='latin1')  # Read the CSV file with specified encoding

# Analyze patterns based on key and mode
# Prepare lists to store keys, modes, and average streams
keys = []  # List to store musical keys
modes = []  # List to store modes (Major/Minor)
average_streams = []  # List to store average streams for each key-mode combination

# Calculate the average streams for each key and mode
for key in data['key'].unique():  # Loop through each unique musical key
    for mode in [0, 1]:  # 0 for Minor, 1 for Major
        subset = data[(data['key'] == key) & (data['mode'] == mode)]  # Filter data for the current key and mode
        if not subset.empty:  # Check if the subset is not empty
            average = subset['streams'].mean()  # Calculate the average streams
            keys.append(key)  # Append the key to the list
            modes.append('Major' if mode == 1 else 'Minor')  # Append the corresponding mode
            average_streams.append(average)  # Append the average streams to the list

# Plotting average streams by key and mode using bar charts
plt.figure(figsize=(10, 6))  # Set figure size
for i in range(len(keys)):  # Loop through keys and modes
    plt.bar(f"{keys[i]} ({modes[i]})", average_streams[i])  # Create a bar for each key-mode combination

plt.title('Average Streams by Key and Mode')  # Title of the plot
plt.xlabel('Key and Mode')  # X-axis label
plt.ylabel('Average Number of Streams')  # Y-axis label
plt.xticks(rotation=45)  # Rotate x-axis labels for better visibility
plt.tight_layout()  # Adjust layout
plt.show()  # Display the plot

# Count artist appearances in playlists and charts
artists = data['artist(s)_name'].unique()  # Get unique artists from the dataset
artist_playlists_count = []  # List to store counts of appearances in playlists
artist_charts_count = []  # List to store counts of appearances in charts

# Count appearances for each artist
for artist in artists:  # Loop through each unique artist
    playlists_count = data[data['artist(s)_name'] == artist]['in_spotify_playlists'].sum() + \
                      data[data['artist(s)_name'] == artist]['in_apple_playlists'].sum()  # Total playlist appearances
    charts_count = data[data['artist(s)_name'] == artist]['in_spotify_charts'].sum()  # Total chart appearances
    
    artist_playlists_count.append(playlists_count)  # Append playlists count to the list
    artist_charts_count.append(charts_count)  # Append charts count to the list

# Create a DataFrame for artist counts
artist_counts = pd.DataFrame({
    'Artist': artists,  # Column for artist names
    'In Playlists': artist_playlists_count,  # Column for playlist counts
    'In Charts': artist_charts_count  # Column for chart counts
})

# Sort and get the top 10 artists in playlists
top_artists_playlists = artist_counts.sort_values(by='In Playlists', ascending=False).head(10)  # Top 10 artists in playlists

# Sort and get the top 10 artists in charts
top_artists_charts = artist_counts.sort_values(by='In Charts', ascending=False).head(10)  # Top 10 artists in charts

# Plot top artists in playlists
plt.figure(figsize=(10, 6))  # Set figure size
plt.barh(top_artists_playlists['Artist'], top_artists_playlists['In Playlists'], color='skyblue')  # Horizontal bar chart
plt.title('Top 10 Artists in Playlists')  # Title of the plot
plt.xlabel('Number of Appearances')  # X-axis label
plt.ylabel('Artist')  # Y-axis label
plt.show()  # Display the plot

# Plot top artists in charts
plt.figure(figsize=(10, 6))  # Set figure size
plt.barh(top_artists_charts['Artist'], top_artists_charts['In Charts'], color='lightgreen')  # Horizontal bar chart
plt.title('Top 10 Artists in Charts')  # Title of the plot
plt.xlabel('Number of Appearances')  # X-axis label
plt.ylabel('Artist')  # Y-axis label
plt.show()  # Display the plot


```

## REFERENCES : 
- ECE2112 Lecture Notes by Prof. Engr. Ma. Madecheen S. Pangaliman, MSc and Prof. Engr. Nico John Leo S. Lobos, MSc, ECE, ECT
- Streamed Spotify Songs 2023 (https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023)
- PYTHON PANDAS TUTORIAL #9 - HOW TO USE VALUE_COUNTS METHOD IN PANDAS. (https://www.youtube.com/watch?v=iYollp0FE_E)
- Seaborn Tutorial : Seaborn Full Course (https://www.youtube.com/watch?v=6GUZXDef2U0)
- Spotify Data Analysis Project | Spotify Data Analysis Using Python | Data Analysis | Simplilearn (https://www.youtube.com/watch?v=8d7ywKCm6HI) 

## AUTHOR :
### DE GUZMAN , JAN DENVER F, 

