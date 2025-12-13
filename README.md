# League of Legends:  Gold Statistical Analysis

**Author:** Roman Respicio

---

## Introduction

League of Legends is a popular multiplayer online battle arena game prevalent in the video game industry and especially the professional esports setting. In the gaming industry, LOL is debated to be one of the most, if not the most, influential and popular video games. The data set I am working with is continuously developed by Oracle's Elixir with match records dated from 2014. It includes many gameplay statistics from a large collection of professional esports LOL matches, making it a valuable source of data representative of the game's highest-level players.

This project investigates the relationship of gold-related metrics to other in-game statistics and outcomes. **Gold** is a significant metric in the game because player performance - which is often measured by damage, kills, assists, survivability, etc - is often predicated by player gold. By allowing for the purchase of in-match items which serve as "power-boosts", gold often shapes how a player will perform on a match-to-match basis. Therefore, the question that this project will explore is **how impactful is total gold to other in-game statistics provided in the dataset?** 

Answering this question is significant because understanding the impact of gold on other player metrics can influence and enhance farming strategies, game objectives/priorities throughout the game, and much more.

### Introduction of Columns

Prior to any cleaning, the data set contains 1,115,664 rows and 164 columns. Each match is represented by 12 columns - 10 player columns (5 for each team) and 2 additional columns that are team-based. 

Here is an introduction to the key columns of focus that will be used to answer the central question to our project... 

- 'gamelength': Column containing integer values that represent the length of each match to the nearest minute. This column will be used to standardize other in-game metrics since those metrics are largely dependent on the length of a game.
- 'position': Denotes the player position in a singular game. Possible values are 'top', 'jng', 'mid', 'bot', and 'sup'. In a singular game, all values are included and only one of each value is included.


There are 929,720 rows and 18 columns in our cleaned data
