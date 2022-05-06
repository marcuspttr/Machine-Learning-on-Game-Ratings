# Project4-ML
# Project Overview
## Goal ##
- The goal of this project is to use data pulled from multiple sources in order to help us predict what factors are most influential to if a player feels positively or negatively about a game that they have purchased.
- The raw data was pulled using APIs from rawg.io and SteamSpy.
- Data evaluated includes genre, publisher, release date, box art colors, critical reception, where the game was available for purchase, copies sold, how many users finished the game, gaming platform, initial price, ESRB rating, and user playtime.

## ETL Process ##

We began with a data set of over 100,000 games, but after some investigation, we shrunk our data set to about 27,000 games. In order to feed our data into a machine learning model, our data set needed extensive cleaning, including:
- Merging Steam and Rawg data sets
- Dropping unnecessary columns
- Removing null values
- Calculating the total positive reviews for each game to find total positive reviews as a % to use as our target.
- Formatting release date column to proper datetime, and then split by release month.
- Declaring lists for each game to allow for each game to store its own unique data.
- Counting and sorting game tags to allow us to ascertain the 25 most popular tags to use as a feature.

## Super Awesome Color Feature ##

Each API call from Rawg.io had a link to the respective games cover art 

Used a K-Means Classifier on each preprocessed image to cluster the dominant colors, then picked 5 dominant colors, which were returned as centroids.
Two approaches were taken:
1) Converted Centroids to color name such as “Green”, “Blue”, “Yellow/Brown” (5 columns)
2) Used Actual RGB Values (15 columns)

Each was then added to the main data set as features, 1 categorical approach, and one numerical.

This was very computationally intensive, taking around 9 hours to download, process, find centroids, and delete from local computer.

## Machine Learning ##

Our target, seen in our data as “review_score” is a measure of what proportion of reviews for the game were positive (similar metric to rotten tomatoes). We had to prepare it by binning the proportion data, making it into categories of 1-5 and one hot encoding that result. This is important for machine learning processes, especially neural networks.

After splitting our data into testing and training sets we:
- Ran a random search parameter hypertuner on a random forest classifier.
- Used the feature select from the forest to run a logisitic regression.
- Dropped low impact columns and created a deep neural network.

## Reflection ##

Our results peaked at an accuracy of 32% for the logisitic regression and 27% for the neural network. An optimistic outlook sees that with 5 possible categories, our models did a good deal better than strictly wild guessing at a games review score.

We had some ideas on some contributing factors to our low results:
- Although our initial data set was large with 100,000 games. These were quickly filtered out as they lacked proper documentation. By the time we merged with the verified games from the Steam API our data had been cut to below 30,000 games.
- A lot of our features were weak: what original platforms games were released on (some that are no longer in circulation such as Neo Geo or 3DO) or what genres or tags a game had (we found many games had a lot of arbitrary tags or some tags such as adventure or action that we overused).

# Directory Structure

**Project4-ML**

**Home**
- .gitignore
- README.md

**Code**
 - ETL.ipynb
 - games_ML.ipynb
 - Color_Extraction.ipynb
- Color_Extraction_Sample.ipynb
- sample_test.ipynb

**Resources**
- Final_Data_w_Colors.csv
- top_1000_from_boris_data.csv

**Notes**
- The Final Data with Colors set was used for the machine learning in this project.
- The Color Extraction and top 1000 data set were the results of an effort to utlize and group the colors in a different way to improve performance.
- Our intial datasets were not uploaded due to size constraints, however, the ETL notebook includes the code to conduct the necessary API pulls.
