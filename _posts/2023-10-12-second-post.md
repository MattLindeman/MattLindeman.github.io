---
layout: post
title:  "How to Use R to Analyze and Visualize College Basketball Data"
author: Matt Lindeman
description: A short post about my favorite R package for NCAA basketball and things to remember when analyzing NCAA basketball
image: "/assets/images/nick-jio-57rD2oDZquc-unsplash.jpg"
---

Analyzing NCAA basketball is unique because of the extremely large amount of players and teams to compare. That consideration makes college basketball both fun and challenging. While this post won't be able to go too in-depth, I will touch on things such as code examples and ideas on how to use the output that will hopefully give everyone a good starting point.

## NcaahoopR

My favorite package to use in R for college basketball data is called [ncaahoopR](https://github.com/lbenz730/ncaahoopR). I've provided a link to the GitHub for the package for anyone interested in learning all the package has to offer. For starters, downloading the package in R takes installing it from GitHub with this code:

```
# install.packages("devtools")
devtools::install_github("lbenz730/ncaahoopR")
```

### Scraping Features

Once the package is installed and loaded into the environement, then you can use the many built in functions that comes with the package. One of the best aspects of ncaahoopR is its ability to scrape date from ESPN. Here are some of the choices of what can be scraped, along with the code to execute them:

* Play-by-play data for a given team and season

```
get_pbp(team, season)
get_pbp("BYU", "2022-23")  # BYU play-by-play data for the 2022-23 season
```

* Play-by-play data for a specific game

```
get_pbp_game(game_ids, extra_parse=TRUE)
get_pbp_game(401470408)  # Play-by-play data for the BYU v Gonzaga Jan 12 2023 game
```

* Get a particular team's roster

```
get_roster(team, season)
get_roster("BYU", "2023-24")  # BYU roster for this upcoming season
```

* Get a particular team's schedule

```
get_schedule(team, season)
get_schedule("BYU", "2022-23")  # BYU 2022-23 schedule, including game ids
```

* Get the schedule for all games on a given date

```
get_master_schedule(date)
get_master_schedule(2023-01-12)  # Schedule for all games on Jan 12 2023
```

* Get the box score for a specific game

```
get_boxscore(game_id)
get_boxscore(401470408)  # Box score for each team of BYU v Gonzaga Jan 12 2023
```

* Get the player stats for a given team and season

```
season_boxscore(team, season = current_season, aggregate = 'average')
season_boxscore("BYU", season = "2022-23")  # BYU player stats for the 2022-23 season
```

The scraping features I use the most out of those is getting play-by-play data and getting player stats for a season. Play-by-play data can be hard to find in a format that is easily searchable and parsed, but the data returned by ncaahoopR is great. The play-by-play data can then be searched to find all the three-point plays, or to find all the plays a particular player was involved in. This can be useful to analyze how often particular plays or events occur or, because the data comes with game time stamps, to analyze timing scenarios such as late game situations. As for having player stats, those are integral because they give an idea of how and where a player is producing when on the court. 

### Visualization Features

The ncaahoopR package also has incredible visualization tools that can make data easier to present and understand. Here are the main visualiztion features, with code and output examples:

* Win probability charts

```
wp_chart_new(game_id, home_col = NULL, away_col = NULL, include_spread = T, show_legend = T)
wp_chart_new(401403405)  # Iowa v Indiana Mar 12 2022
```

![Figure](/assets/images/wp_chart_new.jpg)

The win probability chart shows where the win probability was for each team for each minute interval of the game. This can be used to see where big shifts in win probability occurred, and then return back to the play-by-play data to learn what occured around that time of the game that could have caused the shift. If something good occurred to shift the probability, then the team can work on replicating that, and vice versa for preventing plays that negatively shifted the probability. 

* Game flow charts

```
game_flow(game_id, home_col, away_col)
game_flow(game_id = 401082669, home_col = "blue", away_col = "navy")  # Kentucky v Duke Nov 6 2018
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/game_flow.jpg"/>

Game flow charts can be used in a similar respect to the win probability because they show how the score changed for each minute interval of the game. Focus on particular minutes to see what can be replicated or prevented in the future.

* Assist networks

```
assist_net(team, season, node_col, three_weights = T, threshold = T, message = NA, return_stats = T)
assist_net(team = "Oklahoma", node_col = "firebrick4", season = c(400989185))  # Oklahoma assist network v Kansas Jan 23 2018
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/oklahoma.jpg"/>

Assist networks are unique because they show how many assists a player gave and/or received in a game. This is useful to see how often particular players are involved in scoring that involves assists. Quantifying a team's assists can help give specific, measurable goals when it comes to improving assists as a team. 

* Shot Charts

```
game_shot_chart(game_id, heatmap = F)
game_shot_chart(game_id = 401168364)  # Dayton v Kansas Nov 27 2019
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/shot_chart.jpg"/>

Shot charts show where shots were taken on the floor and whether it was a make or a miss. This is useful in understanding a team's shot tendencies and where shooting strengths and weaknesses lie. This shows where improvements can be made or where defensive adjustments can be made.

## Summary and Considerations

Understanding how to use the ncaahoopR package can give anyone a great start into analyzing and visualizing college basketball data. However, I mentioned earlier that college basketball is unique because of the great number of teams and conferences. The teams and conferences come with varying skill levels and difficulties of opponents. Analyzing data related to a specific team or a specific conference isn't too difficult because strength of schedule and conference differences don't really need to be accounted for. When moving to comparisons or building predictive models between conferences and between 100s of players, that's when things become more complicated. Player stats need to be weighted based on competition, win and losses need to be weighted based on oppenent strength, and player talent constantly needs to reevaluated. Recent changes to the transfer portal and the introduction of NIL deals have also thrown analytics into a new realm of evaluating transfer strength and NIL worth. For now, though, those consideration can be put aside as you start with the basics of ncaahoopR. 
