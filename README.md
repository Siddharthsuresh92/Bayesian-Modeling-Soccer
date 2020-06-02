# Bayesian Modeling - Evaluate Tactical Styles in Soccer

This project analyzes different tactical styles using soccer stats and how each of those styles impact the final position of a team in the English Premier League.

## Data and Timeline
This project uses data from the 2006-07 season till the 2017-18 season of the English Premier League. The data was acquired from Kaggle in the form of 2 different tables: (i) Results: This contains the details such as teams, goals, result and season for each match played during the specified timeline, & (ii) Stats: This contains stats (wins, losses, corners, free kicks etc.) for each team in each of the seasons.

## Motivation
The motivation to undertake this project stemmed from the fact that certain tactical styles are dominant and help teams finisher high in the final standings. Albeit the players who execute the tactical styles are extremely important, more often than not it has been observed that just building up a team with high quality players is never enough for the team to perform well. The coaches and their tactical strategies plays a big role in a team's success. That is why often when a team isn't doing well and a new manager comes in, there are changes to the tactical style and the team either does better or worse, depending on the prowess of the tactical style.

Hence, there is a need to understand which tactical styles do impact such shifts in positions of teams from one year to the other. That is, to understand the reason for a team to drop/rise in the final standing of the league table from one season to the other. This reason could be attributed to a change in tactical style, which could further be explained by the change in overall stats of the team in a given season.

For instance, one question that could be answered through this analysis would be: Does an increase in the number of passes (which represents a possession based tactical style) between two consecutive seasons for a team end up increasing its final position in the league as compared to the previous season? These questions when answered could be the key for coaches and assistant coaches to know what drills to focus on and design the training program for players accordingly.

## Analysis and Model
The data was pre-processed to such that the "changes" in stats are obtained for each team starting from the 2006-07 season till the 2017-18 season. This helped in recording the changes in tactical styles rather than having the absolute number of any of the stats. The next thing was to divide up the EPL table into standing classes. The 4 standing classes used in this analysis were:
1. Champions League Qualification (top 4 teams)
2. Top half (teams #5 to #10)
3. Bottom half (teams #11 to #17)
4. Relegation zone (bottom 3 teams)

The response variable was then created such that it recorded the shifts for a team within these standing classes. 
1. If a team stayed in the same standing class as the previous season - Label: 0
2. If a team moved to any of the higher standing classes - Label: +1
3. If a team moved to any of the lower standing classes - Label: -1

While this problem could have been modeled using any machine learning model, it was better to use a Bayesian approach due to limited size of the data. Furthermore, this analysis gives us a possible distribution of the coefficient in addition to a maximum a posteriori estimate. This approach helps in getting probabilistic estimates of how the changes in tactical styles (via the overall season statistics) impact the shifts in standing class.

The three models tested were:
1. Complete pooling model using an Ordered Logistic Regression with 9 features
2. Complete pooling model using an Ordered Logistic Regression with 9 features and all possible interaction terms amongst those 9 features
3. Partial pooling model (Hierarchical model) by providing a distribution for the priors of the priors of the final model, i.e each prior of the final model will have its own set of priors defined  

WAIC was used to decide the best model out of the three.

## Results
Only 3 features turned out to be significant:

1. goals conceded: -ve coefficient (if a team concedes more goals as compared to the last season, the team might finish in a lower standing class), this shows that keeping clean sheets is an important thing for coaches to consider while devising strategies

2. Shots: +ve coefficient (if a team takes more shots as compared to the last season irrespective of it converting into a goal, the team might finish in a higher standing class), this shows that teams that usually have an aggressive style of play with more shots taken usually end up doing better.

3. Crosses: -ve coefficient (if a team crosses more than it did last season, the team might finish in a lower standing class), this was one of the most interesting finds as it suggests that crossing hurts a team more than it benefits. Crosses are usually considered an aggressive tactic and is quite a technical move. However, based on the data and model it seems to be the other way round. This was later found to be the case in Manchester City's poor performances in late 2019 (https://soccer.nbcsports.com/2019/12/08/manchester-city-is-panicking/).

Such results do help teams make better informed decisions on tactical play. Case in point the Manchester City team. Had they known that data shows crosses to be ineffective, they might have invested in other tactical plays that use the dribbling and shooting skills of its players rather than spending so much time and effort in developing a tactic that doesn't give results.