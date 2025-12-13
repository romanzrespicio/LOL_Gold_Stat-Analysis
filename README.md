
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

<div class="table-wrapper" markdown="1">

|   gamelength | position   |   result |   kills |   deaths |   assists |   damagetochampions |   minionkills |   monsterkills |   totalgold |   total cs |   goldat25 | gold_bins   |
|-------------:|:-----------|---------:|--------:|---------:|----------:|--------------------:|--------------:|---------------:|------------:|-----------:|-----------:|:------------|
|         1924 | top        |        1 |       3 |        0 |        13 |               12569 |           209 |              0 |       11499 |        209 |       8735 | 9k-12k      |
|         1924 | jng        |        1 |       0 |        4 |        14 |                7067 |            24 |             91 |       10113 |        115 |       7122 | 9k-12k      |
|         1924 | mid        |        1 |      10 |        1 |         7 |               27699 |           247 |             29 |       14564 |        276 |       9312 | 12k-15k     |
|         1924 | bot        |        1 |       8 |        0 |         9 |               17296 |           250 |             32 |       14210 |        282 |      10279 | 12k-15k     |
|         1924 | sup        |        1 |       0 |        0 |        15 |                9217 |            11 |              0 |        9045 |         11 |       6277 | 9k-12k      |

</div>

The cleaned dataset contains only the relevant columns which will be used in **hypothesis testing**, testing for **missingness**, and the **prediction model**. Depending on the type of analysis, the dataframe will be further altered to comply to any necessary steps (e.g. one-hot encoding the position column for prediction model).

## Univariate Analysis - Gold

Univariate analysis was first performed on the 'totalgold' column of our dataset.

<iframe
  src="assets/unigoldanal.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

The plotted histogram of total gold earned in a game shows that the distribution of gold is nearly normal with a slight right skew. The normal spread reveals that the data is balanced and follows the Central Limit Theorem.

## Univariate Analysis - Damage to Champions

Next, we perform the same analysis on the 'damagetochampions' column of the dataset.

<iframe
  src="assets/unigoldanal2.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

The plotted histogram shows that total damage to enemy champions is not normally distributed. The data appears to have a right skew, suggesting the data may not be well-behaved.

## Bivariate Analysis - Gold and Damage 

Bivariate analysis was performed on the 'gold_bins' column and the 'damagetochampions' column to explore the relationship between increasing levels of gold with the distribution of damage to enemy champions. This analysis was done by generating side-by-side box plots of each gold bin category. Gold amounts were binned into 3k gold ranges from 0 to 30k, then 30k+ gold amounts.

<div style="width: 800px; max-width: 100%; overflow: hidden; margin: 0 auto;">
  <iframe
    src="assets/bianalgolddam.html"
    width="1000"
    height="600"
    frameborder="0"
    style="
      transform: scale(0.8);
      transform-origin: 0 0;
      width: 1250px;  /* 1000 / 0.8 */
      height: 750px;  /* 600 / 0.8 */
      border: none;
    "
  ></iframe>
</div>

The box plot appears to reveal increasing measures of center of damage as gold ranges increase. This suggests a positive correlation between gold and damage to enemy champions, which poses gold as an important statistic that is important to player performance when measured by damage. However, it is also important to note that there are many outliers, especially in the higher ranges of damage, that are past the upper fence of each box plot. The presence of these outliers implies that gold is not the only determining factor of damage to champions. It is also worth noting that the spread of the data as bins increase seem to increase as well.

## Aggregate Analysis

Here are some interesting aggregates in the dataset...

<div class="table-wrapper" markdown="1">

|   Avg Damage |   Avg Kills |   Avg Deaths |   Avg Assists |   minionkills |   monsterkills |
|-------------:|------------:|-------------:|--------------:|--------------:|---------------:|
|       1039.6 |         0.1 |          0.9 |           0.3 |          19.1 |            3.6 |
|       1063   |         0.2 |          0.2 |           0.8 |          22.2 |            4.5 |
|       3843.9 |         0.3 |          4.1 |           2.8 |          31.7 |            3.7 |
|       3636.4 |         0.5 |          1.5 |           9   |          27.9 |            0.8 |
|       6871.6 |         0.8 |          4.1 |           3.8 |          79.7 |           29.2 |
|       5608.1 |         1   |          1.9 |          11.6 |          38   |            7   |
|      11803.4 |         1.7 |          3.8 |           3.7 |         167.2 |           41.7 |
|      10768.6 |         2.7 |          1.8 |           9.4 |         107.2 |           50.6 |
|      17409.2 |         2.8 |          3.6 |           4.5 |         241.3 |           36.9 |
|      16895.5 |         4.4 |          1.8 |           8.1 |         197.1 |           48   |
|      23951   |         4   |          3.6 |           5.4 |         301.8 |           36.6 |
|      23058.3 |         5.8 |          2   |           7.8 |         269.7 |           40.2 |
|      31297.2 |         5.2 |          3.5 |           6.1 |         357.7 |           40.3 |
|      30141.4 |         6.9 |          2.3 |           7.7 |         332.9 |           41.1 |
|      38407.2 |         6.1 |          3.4 |           6.5 |         413.7 |           45.6 |
|      38082.6 |         7.7 |          2.5 |           7.9 |         390.7 |           45.8 |
|      45769.5 |         7.2 |          3.6 |           7.1 |         469.2 |           48.8 |
|      46290.1 |         8.2 |          2.7 |           7.9 |         449.3 |           53.4 |
|      49066.8 |         6.7 |          3   |           6.9 |         531.5 |           54.6 |
|      54188.4 |         8   |          2.9 |           8.2 |         514.2 |           62.2 |
|      64872.9 |         7.4 |          2.9 |           7.2 |         646.5 |           71.8 |
|      54723.9 |         6.6 |          2.9 |           7   |         623.7 |           66   |

</div>

This aggregate data table groups the dataset by gold-range bins and game result (0 indicating a loss and 1 indicating a win), then computes the mean values of each of the shown variables. Looking at the aggregated statistics, one can observe that as the gold range increases, values of average damage, average kills, minionkills, and monsterkills seem to rise. The columns, average deaths and average assists, do not seem to clearly show this same pattern just looking at the computed means. There does not seem to be significant differences in values between games that are lost versus won.

### Assessment of Missingness

## NMAR Analysis

In the original dataset, the ban columns - 'ban1', 'ban2', 'ban3', 'ban4', 'ban5' - are Not Missing at Random (NMAR). Before each match, each League of Legends 'draft' game includes a character pick phase and a ban phase. During this ban phase, players themselves decide who to ban or perhaps even choose whether they want to ban a character at all. Though unlikely during professional play, occasionally a player may choose not to ban a character at all. Thus, the missingness of values in these columns depends on the column itself. Additional data that may make this data MAR is a column that indicates whether two players on the same team tried to ban the same character. In this case, one of the players with the same ban on the same team would not have data in their ban slot. 

## Missingness Dependency

In this section, we are going to test the missingness of the 'goldat25' column. We are assessing if this column is dependent on the columns 'gamelength' and 'result'. The significance level chosen for both permutation tests will be the standard 0.05.

**Null Hypothesis:** The distribution of 'gamelength' when 'goldat25' is missing is the same when 'goldat25' is not missing.

**Alternative Hypothesis:** The distribution of 'gamelength' when 'goldat25' is missing is **not** the same when 'goldat25' is not missing.

To test the hypotheses, the test statistic chosen is the Kolmogorov-Smirnov statistic.

<iframe
  src="assets/permtest1.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

One can observe from the graph that the distributions are not the same. Furthermore, we have a calculated p-value that is approximately 0 and less than our significance level of 0.05. Therefore, we **reject the null hypothesis** in favor of the alternative hypothesis. Thus, 'goldat25' seems to be Missing at Random with dependency on the 'gamelength column.

Next, we perform another permutation test to test 'goldat25' column missingness dependency on the 'result' column.

**Null Hypothesis:** The missingness of 'goldat25' is independent of 'result'.

**Alternative Hypothesis:** The missingness of 'goldat25' is dependent on 'result'.

To test the hypotheses, the test statistic chosen is the absolute difference in means.

<iframe
  src="assets/permtest2"
  width="800"
  height="450"
  frameborder="0"
></iframe>

Our calculated p-value appears to be 0.332 from the permutation test. Since this value is > our significance level of 0.05, we **fail to reject** our null hypothesis. The missingness of 'goldat25' does not appear to be MAR dependent on the 'result' column

### Hypothesis Testing

The goal of our hypothesis test is to explore the possibility of a significant difference in average damage between players who accumulate "low" amounts of gold (less than or equal to 15k) versus players who accumulate "high" amounts of gold (over 15k). This test is significant because it potentially uncovers the impact of gold on other game-related statitics and potentially game outcome. This hypothesis test will be run with a significance level of 0.05.

**Null Hypothesis:** The average champion damage is the same between players who accumulated "high" amounts of gold versus players who accumulated "low" amounts of gold.

**Alternative Hypothesis:** The average champion damage is higher among players who accumulated "high" amounts of gold than players who accumulated "low" amounts of gold.

**Test Statistic:** Signed mean difference of average champion damage (players with "high" amounts of gold - players with "low" amounts of gold.

Running the permutation test reveals that the observed signed mean difference of damage (players accumulating "high" - "low" amounts of gold) was 14,195.59 damage points. The resulting p-value is approximately 0 and less than our chosen significant level. Therefore, we **reject the null hypothesis** in favor of the alternative hypothesis. Players who accumulate "high" amounts of gold (over 15000) may do more damage than players who accumulate "low" amounts of gold (less than or equal to 15000).


