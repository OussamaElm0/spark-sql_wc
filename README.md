# FIFA World Cup Analysis with PySpark

## Project Overview
This project analyzes the FIFA World Cup match data using PySpark to extract various insights and statistics. The dataset contains historical football match results, including information on home and away teams, scores, tournament details, dates, and locations. The analysis covers aspects such as team performance, goal statistics, tournament trends, and more.

## Table of Contents
1. [Setup and Installation](#setup-and-installation)
2. [Data Loading and Overview](#data-loading-and-overview)
3. [Data Exploration](#data-exploration)
4. [Aggregations & Statistics](#aggregations--statistics)
5. [Analytical Queries](#analytical-queries)

## Setup and Installation
To run this notebook, you need to have Apache Spark and PySpark installed and configured. The initial cells in this notebook handle the installation and setup within a Colab environment.

```python
!sudo apt update
!apt-get install openjdk-8-jdk-headless -qq > /dev/null
!wget -q https://archive.apache.org/dist/spark/spark-4.1.0/spark-4.1.0-bin-hadoop3.tgz
!tar xf spark-4.1.0-bin-hadoop3.tgz
!pip install -q findspark
!pip install pyspark
!pip install py4j
!pip install -q pymongo matplotlib seaborn

import os
import sys
import findspark
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-17-openjdk-amd64"
os.environ["SPARK_HOME"] = "/content/spark-4.1.0-bin-hadoop3"
findspark.init()

import pyspark
from pyspark.sql import SparkSession

spark = SparkSession.builder \
  .appName("Fifa WC") \
  .config("spark.driver.memory", "1G") \
  .getOrCreate()
print("Spark est configuré avec succès!")
```

## Data Loading and Overview
The dataset `fifaworldcup.csv` is loaded from Google Drive into a PySpark DataFrame. Basic commands are used to display the schema and a sample of the data.

```python
from google.colab import drive
drive.mount("/content/drive")

df = spark.read.csv("/content/drive/MyDrive/Colab Notebooks/Py Spark/fifaworldcup.csv", header=True, inferSchema=True)
df.show()
```

## Data Exploration
This section involves basic exploratory data analysis to understand the dataset's characteristics.

- **Number of Matches:** The total count of matches in the dataset.
- **Time Range:** The first and last years covered by the data.
- **Frequent Tournaments:** Identifies the top 10 most frequent tournaments.
- **Neutral Venue Matches:** Counts the number of matches played on a neutral ground.
- **Top Hosting Countries:** Lists the top 10 countries that have hosted the most matches.
- **Draw Matches:** Calculates the number of matches that ended in a draw.
- **High-Scoring Matches:** Filters and displays matches where the total score (home_score + away_score) was 6 or more goals.

## Aggregations & Statistics
This part focuses on performing various aggregations to derive meaningful statistics.

- **Total Matches per Team:** Calculates the total number of matches played by each team (both home and away).
- **Top Goal-Scoring Teams:** Identifies the top 10 teams that have scored the most goals across all competitions.
- **Average Goals per Decade:** Computes the average number of goals per match, grouped by decade, to observe trends.
- **Matches per Tournament per Year:** Shows the number of matches played for each tournament in each year.
- **Top Home Winners:** Lists the top 10 teams with the most wins when playing at home.
- **Team Performance Summary:** Provides a breakdown of wins, losses, and draws for each team.
- **Neutral vs. Non-Neutral Venue Scores:** Compares the average total goals in matches played at neutral venues versus non-neutral venues.
- **Highest Score Difference:** Shows the top 5 matches with the largest goal difference between the winning and losing teams.

## Analytical Queries
This section delves into more advanced analytical queries using window functions and conditional logic.

- **Goal Average (Goal Difference):** Calculates the goal difference (goals for - goals against) for each team.
- **Team Ranking by Annual Wins:** Ranks teams by the number of wins they achieved each year.
- **Evolution of Matches per Decade:** Tracks the total number of matches played over different decades.
- **Undefeated Teams:** Identifies teams that remained undefeated in a specific year.
- **Longest Winning Streaks:** Determines the longest consecutive winning streak for each team.
- **Most Victorious Team per Tournament:** Finds the team with the most wins in each specific tournament.
- **Home vs. Away Performance:** Provides a detailed breakdown of each team's performance (wins, losses, draws) when playing at home, away, and at neutral venues for each tournament.