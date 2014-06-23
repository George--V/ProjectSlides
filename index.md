---
title       : World Cup 2014 Probabilities
subtitle    : Find what the expectations are before the game
author      : GV
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides

---

### What does this application do?

* This application allows you to find some interesting statistical infomation about the teams playing the world cup 2014.

* You simply need to input any two teams, which can be selected from a list, and press the submit button.

* You will then be presented with the following:

1) The current points of both countries in the FIFA ranking.

2) The probabilities of the outcome if they play each other.

The application can be found here: http://george--v.shinyapps.io/ProjectDevDataProd 

---

### What is the FIFA ranking ?

* It's a point system used by the Internationation Football Association
* It's based on statistics that takes into account every international game in the last four years.
* The points each team gets in a match are calculated as follows:

###### Points= M * I * T * C  
 

  M (winning, drawing or losing a match)
  I (importance of the match)
  T (strength of opposing team)
  C (confederation strength)

The total points for each team in the FIFA ranking are then weighted in relation to the last for years:

For more information visit: http://www.fifa.com/worldranking/rankingtable/index.html

---

### How are the probabilites calculated?

We use the following reasoning to come up with the algorithm:

* There are 3 possible results in football (win, lose or draw)
* In a totally random environment the probability of these events would be  1/3 (roughly 33%).
* If we base our model on FIFA rankings then we should skew the distribution towards the higher ranked team proportianally to its relation to the lower ranked team.

We then come up with this formula (w=higher ranked, l=lower ranked):

* Higher ranked team to win: 
  probwin = 34+((66/50) * ((w/(w+l))*100 -50))
* Draw:
  probdraw= (100-probwin) * w/(w+l))
* Lower ranked to win:
  problose= (100-(probwin+probdraw))


---  

#### Example in R: Uruguay (1147 points) vs. England (1090 points)


```r
w <- 1147
l <- 1090
probwin <- as.integer(34 + ((66/50) * ((w/(w + l)) * 100 - 50)))
probdraw <- as.integer((100 - probwin) * w/(w + l))
problose <- as.integer(100 - (probwin + probdraw))
paste("Uruguay to win: ", probwin, "%")
```

```
## [1] "Uruguay to win:  35 %"
```

```r
paste("England to win: ", problose, "%")
```

```
## [1] "England to win:  32 %"
```

```r
paste("Draw", probdraw, "%")
```

```
## [1] "Draw 33 %"
```


