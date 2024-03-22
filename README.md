# Pick-Ban-Analysis

## Introduction
My dataset is on all professional League of Legends games in 2022 from Oracle's Elixir. My dataset has 131 columns and 148992 rows, with 12 games per row. 10 of these rows contain player data, while the two rows beneath contain overall team data. The research question I'd like to answer is 'Are pick-ban patterns different for winning versus losing teams?'


Before professional games start in League of Legends, there is a pre-game session known as Champion Select. Initially, each team is allotted three bans, after which they proceed to select three champions each. Following these initial picks, each team is granted an additional two bans. To conclude the selection process, both teams pick their final two champions. There's a lot of nuance in Champion Select, as each team must choose between grabbing "high-priority" champions—those highly sought after by both teams for their strength—and waiting to pick later in the order to "counter-pick." Counter-picking means selecting champions that specifically counter the abilities or strategies of the opponent's already chosen champions. This decision requires balancing the benefit of securing powerful champions early against the advantage of responding directly to the opponent's lineup with effective answers.


Champion Select is often emphasized by analysts, commenting that games are "won" or "lost" in Champion Select–before the game has even started!! 


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

I first filtered the dataframe to only include our relevant columns. Then, I dropped null rows with null values for picks and bans, since I felt as if I was unable to impute values in a way that would make sense, or use the null values in my analysis. I also prepared my data for some EDA I planned on conducting–by mapping each champion pick and ban to the primary position that they played in through a dictionary and storing them in new columns. This dictionary was inputted manually but found algorithmically–champions that were not picked for a singular position >= 60% of the time were not mapped. Then, I aggregated the dataframe based on `gameid` and `side`, since I wanted each game to only be represented by two rows–one for each team. 


The transposed head of the dataframe:

| Attribute | 0                     | 1                    | 2                     | 3                    | 4                     |
|-----------|-----------------------|----------------------|-----------------------|----------------------|-----------------------|
| gameid    | 8401-8401_game_1 Blue | 8401-8401_game_1 Red | 8401-8401_game_2 Blue | 8401-8401_game_2 Red | 8402-8402_game_1 Blue |
| teamname  | Oh My God             | ThunderTalk Gaming   | Oh My God             | ThunderTalk Gaming   | FunPlus Phoenix       |
| ban1      | Renekton              | Samira               | Renekton              | Samira               | LeBlanc               |
| ban2      | Lee Sin               | Diana                | Caitlyn               | Diana                | Twisted Fate          |
| ban3      | Caitlyn               | Akali                | Thresh                | Jarvan IV            | Aphelios              |
| ban4      | Jayce                 | LeBlanc              | Jayce                 | LeBlanc              | Nautilus              |
| ban5      | Camille               | Rumble               | Camille               | Akali                | Leona                 |
| pick1     | Jinx                  | Xin Zhao             | Jinx                  | Lee Sin              | Jinx                  |
| pick2     | Jarvan IV             | Thresh               | Xin Zhao              | Leona                | Viego                 |
| pick3     | Nautilus              | Aphelios             | Rakan                 | Ziggs                | Thresh                |
| pick4     | Syndra                | Vex                  | Rumble                | Gangplank            | Corki                 |
| pick5     | Gwen                  | Jax                  | Corki                 | Twisted Fate         | Graves                |
| league    | LPL                   | LPL                  | LPL                   | LPL                  | LPL                   |
| patch     | 12.01                 | 12.01                | 12.01                 | 12.01                | 12.01                 |
| result    | 1                     | 0                    | 1                     | 0                    | 1                     |
| ban1_pos  | top                   | adc                  | top                   | adc                  | mid                   |
| ban2_pos  | top                   | jg                   | adc                   | jg                   | mid                   |
| ban3_pos  | adc                   | mid                  | sup                   | jg                   | adc                   |
| ban4_pos  | top                   | mid                  | top                   | mid                  | sup                   |
| ban5_pos  | top                   | top                  | top                   | mid                  | sup                   |
| pick1_pos | adc                   | adc                  | adc                   | top                  | adc                   |
| pick2_pos | jg                    | sup                  | adc                   | sup                  | jg                    |
| pick3_pos | sup                   | adc                  | sup                   | adc                  | sup                   |
| pick4_pos | mid                   | mid                  | top                   | top                  | mid                   |
| pick5_pos | top                   | top                  | mid                   | mid                  | top                   |

**Univariate Analysis**


<iframe
  src="assets/avg_pick_order_by_pos.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Barplot of the average pick order by position generated with some dataframe manipulation. It looks like Adc and Jg are typically picked the earliest, followed by Support, Mid, and Top. 


**Bivariate Analysis**

<iframe
  src="assets/pick_order_by_pos_win_vs_loss.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


Distribution of Pick Orders by Position of Winning vs. Losing teams, visualized by a Box and Whisker plot. It looks like winning teams have a higher Q1, making more of their data centered around a later pick order for Top. 


**Interesting Aggregates**



| result | top      | jg       | adc      | mid      | sup      |
|--------|----------|----------|----------|----------|----------|
| 0      | 1.085124 | 0.741736 | 1.058099 | 1.250579 | 0.720248 |
| 1      | 1.052280 | 0.801930 | 1.015668 | 1.241115 | 0.733817 |


This aggregate shows the average number of bans each role recieves in champ select for winning versus losing teams. It appears as if Top is banned out less in winning teams, while Jg and Adc are banned out more.


## Assessment of Missingness


I believe that many columns in this dataset are Not Missing At Random (NMAR). For example, all 3 bans are missing for many of the rows yet still contain team data, making it unlikely that the reason for its missingness can be explained by design(MD). Additionally, data is missing in chunks in the dataset, making it unlikely that the missingness is Missing Completely at Random(MCAR) and likely that it's dependent on the type of column the data is stored in.


Additional data that would make bans Missing at Random(MAR) can be collected from the `league` column. Smaller leagues should have more missing data. For simplicity, I used `ban1` as my column of interest. I plotted an Empirical Distribution of TVDs found through shuffling the `ban1` column under the assumption that missingness of `ban1` is not dependent on `league`. Our 


<iframe
  src="assets/TVDs_ban_vs_league.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


p-value: 0.000


**Missingness of `ban1` is dependent on `league`.**


The same test done on `side`.


<iframe
  src="assets/TVDs_ban_vs_side.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


p-value: 0.124



**Missingness of `ban1` does not depend on `side`.**


## Hypothesis Testing

I conducted 5 separate permutation tests, one for each role. I wanted to see if the difference in ban average number of bans between winning and losing teams that I found in my EDA could be explained by chance. 

**Null Hypothesis**: The average number of bans for each role is the same between winning and losing teams

**Alternate Hypothesis**: The average number of bans for each role is different between winning and losing teams

**Test Statistic**: The difference in means 


I chose to use the difference in means because there are two populations at interest–winning and losing teams. 


**Significance Level**: 0.05


I chose 0.05 because it is the most common threshold for hypothesis tests.


**p-value**: 0.0071(top), 0.0000(jg),  0.4776(mid), 0.0009(adc), 0.2218(sup)


**Conclusion**: We reject the null hypothesis for Top, Jg, and Adc. We don't believe average number of bans for these roles for winning teams come from the same distribution as losing teams. We fail to reject the null hypothesis for Mid and Sup.


<iframe
  src="assets/permutated_differences_top.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

p-value : 0.0071


<iframe
  src="assets/permutated_differences_jg.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

p-value : 0.0000


<iframe
  src="assets/permutated_differences_mid.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

p-value : 0.4776


<iframe
  src="assets/permutated_differences_adc.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

p-value : 0.0009


<iframe
  src="assets/permutated_differences_sup.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

p-value : 0.2218


## Framing a Prediction Problem

My prediction problem is to determine the result of the game strictly from pre-game information. This is a binary classification problem, as I'll need to predict the response variable: either a win(1) or a loss(0). I'll use the KNNClassifier provided by Sci-kit learn as our base model. Additionally, I'll use accuracy as my scoring metric since false positives and false negatives don't make a difference.

## Baseline Model

Our baseline model uses a KNN Classifier with a OneHotEncoded `side` and `pick1` variable. These are both nominal variables. Our model achieved an accuracy of 0.511, which isn't good and is barely better than guessing. 

## Final Model

For our final model, I selected `pick4`, `pick5`, `side`, `patch`, and `teamname` to be my parameters. I only chose `pick4` and `pick5` to avoid multicollinearity and to reduce noise. I chose `teamname` because teams that are historically better should win more, and `side` because it influences strategy. `Patch` is also important, since champion strength and playstyle are affected by each update. 


I OneHotEncoded all of these variables, since they are all nominal. I considered an ordinal encoding for `patch` but decided against it because gaps between patches should not be consistent, since there are routinely big and small updates, along with new seasons. 


Additionally, I transformed all my variables with MCA, a dimensionality reduction technique for categorical variables. I did this by OneHotEncoding my variables, subtracting the column-wise mean, then applying SVD. I found the dimensions that would let me keep around 95% of the variance in the original dataset.

I originally stuck with my KNN Classifier, tuning its hyperparameters to give me the best result. 

<iframe
  src="assets/grid_search_heatmap.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

However, I found that Support Vector Classification(SVC) gave me a better result out of all the models I fitted. I fit it with the regularization parameter of 1.1 and I got an accuracy of 0.5952799397439116. I attempted to run GridSearch on it, but unfortunately I don't have a system powerful enough to run it. Still, its an improvement over my baseline model by nearly 10 percent.

## Fairness Analysis


