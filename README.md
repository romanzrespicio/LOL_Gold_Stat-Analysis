**Author:** Roman Respicio

---

## Introduction

League of Legends is a popular multiplayer online battle arena game prevalent in the video game industry and especially the professional esports setting. In the gaming industry, LOL is debated to be one of the most, if not the most, influential and popular video games. The data set I am working with is continuously developed by Oracle's Elixir with match records dated from 2014. It includes many gameplay statistics from a large collection of professional esports LOL matches, making it a valuable source of data representative of the game's highest-level players.

This project investigates the relationship of gold-related metrics to other in-game statistics and outcomes. **Gold** is a significant metric in the game because player performance - which is often measured by damage, kills, assists, survivability, etc - is often predicated by player gold. By allowing for the purchase of in-match items which serve as "power-boosts", gold often shapes how a player will perform on a match-to-match basis. Therefore, the question that this project will explore is **how impactful is total gold to other in-game statistics provided in the dataset?** 

Answering this question is significant because understanding the impact of gold on other player metrics can influence and enhance farming strategies, game objectives/priorities throughout the game, and much more.

### Introduction of Columns

Prior to any cleaning, the data set contains 1,115,664 rows and 164 columns. Each match is represented by 12 columns - 10 player columns (5 for each team) and 2 additional columns that are team-based. 

Here is an introduction to the key columns of focus that will be used to answer the central question to our project... 

- 'gamelength': Column containing integer values that represent the length of each match to the nearest minute.
- 'position': Denotes the player position in a singular game. Possible values are 'top', 'jng', 'mid', 'bot', and 'sup'. In a singular game, all values are included and only one of each value is included.
- 'result': Column indicating whether an individual player 'won' or 'lost' the specific match. Contains binary values where 1 indicates a match win and 0 indicates a match loss.
- 'kills': Counts the number of enemy champions killed in a particular match by a particular player. Contains integer values.
- 'deaths': Counts the number of occurrences where a player is killed (either by enemy champions, minions, or turrets) in a particular match. Contains integer values.
- 'assists': Counts the number of occurrences where a player contributes to the kill of an enemy champion without securing the kill themselves. Contains integer values.
- 'damagetochampions': Measures the amount of damage an individual player delivers to enemy champions (opponents). This column contains floating-point values.
- 'minionkills': Counts the number of minions that a particular player eliminates in a single match. The column contains integer values.
- 'monsterkills': Counts the number of monsters (rather than minions, spawns in the jungle of the map) that a player kills in a particular match. Contains integer values.
- 'totalgold': Measures the total amount of gold a player earns regardless of how it is earned (champion kills, minion kills, monster kills, turret plating, etc). The column contains floating-point values.
- 'total cs': Measures the 'creep score' of a player, which refers to the total number of minions and jungle monsters that a player eliminates. 'cs' is often used as a measure of farm. Contains floating-point values.
- 'goldat25': Measures the total amount of gold a player reaches at 25 minutes of the game. Contains floating-point values.
- 'gold_bins': Column created based on 'totalgold' to cut the dataset into gold range categories of 3k (0-3k, 3k-6k, etc) with the right-hand value not being included. This column includes strings that represent categories later used in bivariate analyses.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
To begin preparing our dataset, we first keep only relevant columns of the original dataset, as well as the 'participantid' column, to drop any rows that do not represent an individual player. In the original dataset, these rows have 'participantid' values of either 100 or 200. After dropping those rows, drop the 'participantid' column. These rows are dropped since our analyses only involve individual player metrics.

After this step of cleaning up the dataset, there are 929,720 rows and 12 columns. However, since scatter plots including nearly a million values of data were very difficult to load/read, a new categorical column was created - 'gold_bins'. To create this column, the 'totalgold' column was cut using pandas into bins of size 3,000, and each player was assigned to their corresponding range.

The cleaned and prepared dataset contains 929720 rows and 13 columns. Below is the head of the dataframe...

|   gamelength | position   |   result |   kills |   deaths |   assists |   damagetochampions |   minionkills |   monsterkills |   totalgold |   total cs |   goldat25 | gold_bins   |
|-------------:|:-----------|---------:|--------:|---------:|----------:|--------------------:|--------------:|---------------:|------------:|-----------:|-----------:|:------------|
|         1924 | top        |        1 |       3 |        0 |        13 |               12569 |           209 |              0 |       11499 |        209 |       8735 | 9k-12k      |
|         1924 | jng        |        1 |       0 |        4 |        14 |                7067 |            24 |             91 |       10113 |        115 |       7122 | 9k-12k      |
|         1924 | mid        |        1 |      10 |        1 |         7 |               27699 |           247 |             29 |       14564 |        276 |       9312 | 12k-15k     |
|         1924 | bot        |        1 |       8 |        0 |         9 |               17296 |           250 |             32 |       14210 |        282 |      10279 | 12k-15k     |
|         1924 | sup        |        1 |       0 |        0 |        15 |                9217 |            11 |              0 |        9045 |         11 |       6277 | 9k-12k      |

