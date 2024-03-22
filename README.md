# Pick-Ban-Analysis

## Introduction
My dataset is on all professional League of Legends games in 2022 from Oracle's Elixir. MY dataset has 131 columns and 148992 rows, with 12 games per row. 10 of these rows contain player data, while the two rows beneath contain overall team data. The research question I'd like to answer is 'Are pick-ban patterns different for winning versus losing teams?'

Before professional games start in League of Legends, there is a pre-game session known as Champion Select. Initially, each team is allotted three bans, after which they proceed to select three champions each. Following these initial picks, each team is granted an additional two bans. To conclude the selection process, both teams pick their final two champions. There's a lot of nuance in Champion Select, as each team must choose between grabbing "high-priority" champions—those highly sought after by both teams for their strength—and waiting to pick later in the order to "counter-pick." Counter-picking means selecting champions that specifically counter the abilities or strategies of the opponent's already chosen champions. This decision requires balancing the benefit of securing powerful champions early against the advantage of responding directly to the opponent's lineup with effective answers.

Champion Select is particularly interesting because its often emphasized by analysts, commenting that games are "won" or "lost" in Champion Select–before the game has even started!! 

The column names I'm interested in are: `gameid`, `teamname`, `side`, `ban1`, `ban2`, `ban3`, `ban4`, `ban5`, `pick1`, `pick2`, `pick3`, `pick4`, `pick5`, `league`, `patch`, and `result`.

**Description of Columns**

`gameid`: Unique identifier for each game

`teamname`: Name of the team participating

`side`: Either Red or Blue, determining the side of the map they play on 

`ban1`, `ban2`, `ban3`, `ban4`, `ban5`: Bans of each team, in order

`pick1`, `pick2`, `pick3`, `pick4`, `pick5`: Picks of each team, in order

`league` : The region

`patch`: The version of the game

`result`: Win(1) or Loss(0)

## Data Cleaning and Exploratory Data Analysis

<iframe
  src="assets/avg_pick_order_by_pos.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/pick_order_by_pos_win_vs_loss.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| result | top      | jg       | adc      | mid      | sup      |
|--------|----------|----------|----------|----------|----------|
| 0      | 1.085124 | 0.741736 | 1.058099 | 1.250579 | 0.720248 |
| 1      | 1.052280 | 0.801930 | 1.015668 | 1.241115 | 0.733817 |

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
