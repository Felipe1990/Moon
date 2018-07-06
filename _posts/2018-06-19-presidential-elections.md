---
layout: post
title: "Visualization of presidential election results in Colombia - 2018"
date: 2018-06-19
excerpt: "Visual analysis of the Colombian presidential elections"
tags: [elections, R, presidential, visualization, ggplot2, dplyr, tidyverse]
comments: true
---

Presidential elections have passed and Duque is the new president. After Registraduria Nacional posted the final results [^1] I collected the figures from its webpage using a program written in Python, which went through all departamentos and municpios (municipalities) collecting the votes cast for each candidate and turnout figures. The resulting database was limited by the results found only in the second round of elections. Thus I complemented it with the results from the first round and the national peace plebiscite held in 2016. 

The aim was to capture the movement of votes through elections rounds and infer 1) what kind of impact political views (captured in the plebiscite results) have on the presidential election 2) whether poverty incidence presentest a correlation with results in the second round.

[^1]: [Results](https://presidente2018.registraduria.gov.co/resultados/2html/resultados.html)

All the data collection was conducted using Python[^2] and the analysis using R[^3].

[^2]: I plan to post about the details on how the data was collected and the code used. Selenium package was used.
[^3]: I find myself prefering the tidyverse right now.

## Peace voters in the first round of 2018

I started by looking at the relation between the share of voters who said "Yes" in the peace plebiscite (2016) and the share of votes for each one of the candidates at a municipal level, disaggregating the results by candidate (Sergio Fajardo, Iván Duque, Humberto de la Calle, Gustavo Petro and Germán Vargas -no particular order-).

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/pleb_firstround_1.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/pleb_firstround_1.jpg" width="400" height="300" align="middle"></a>
</figure>

As one could expect there is high (negative) relation between the votes cast for "Yes" in the plebiscite and for Iván Duque in the first round, actually the slop is pretty steep and without much variance. Municipalities where Duque and the right is strong seem to be close together in their electoral behaviour. On the other side, for the candidates who said will defend the peace accords the municipalities's support seem to be disperse. Interestingly, Fajardo seems to have a maximun of support after which the graphs suggest municipalities will prefer Petro (or Vargas Lleras). Unfortunately for Humberto de la calle support appears to be consistently low.

Now lets divide the aforesaid candidates into three groups pro-peace-accords (Sergio Fajardo, Gustavo Petro, Humberto de la Calle), anti-peace-accords (Iván Duque) and DK/DA-peace-accords (Germán Vargas Lleras) and aggregate the results for each one.

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/pleb_firstround_2.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/pleb_firstround_2.jpg" width="400" height="300" align="middle"></a>
</figure>

What appears is an interesting and  self-explanatory image of a divided country. Both smooth lines[^4] for the pro-peace-accords group and anti-peace-accords group are pretty steep.

[^4]: Smoothing method: Loess

## Movement between rounds

First round was over and differences between candidates policies and vision was clear. Fajardo was out and the final round was between Duque and Petro. The big question before the election was, what would the Fajardo voters do? would they support Petro (vote againts Duque) or go for the white vote.

As a visual aproximation for what happened after the elections I thought a Sankey graph could provide some insights. For the plot I calculated the number of municipalities where each candidate won (had more votes than the other candidates). Hence width of "bars" represents how many municipalities the candidate won.

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/sankey_frvsr.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/sankey_frvsr.jpg" width="400" height="300" align="middle"></a>
</figure>

It's interesting to notice that both Duque and Petro lost some municipalities (Petro more than Duque). In addition, those where Fajardo won were close to evenly divided between Petro and Duque. This, off course, didn't help Petro. The plot suggests that the municipalities that weren't won by Petro or Duque in the first round did't chose a main candidate for the second round.

## White vote

But, what happened with the white vote? Fajardo and other politicians were in favor of voting white and an expectative of a high share of the white vote rised. Lets (safely) assume that white voters would come from Fajardo's votes, but what the graph shows is that even though there is a positive relationship between share of vote for Fajardo an white vote, it is weak. The final election results show that white voters could not have had any incidence in the result should they have voted for Petro.

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/distribution_fcsround.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/distribution_fcsround.jpg" width="400" height="300" align="middle"></a>
</figure>

## Poverty and votes for Petro in the second round

I was looking for alternative ways of presenting the results and found spyder plots to be an interesting approach to try.

The idea behind this plot is the following. Departamentos are orderd clockwise (starting by Bogotá) by poverty incidence[^5], black line represent the share of votes received by Gustavo Petro in the second round of 2018 presidental elections in each department and red line represents the national share votes. For example, one could interpret that the second departamento with the lowest poverty incidence (Cundinamarca) had a share of votes for Petro below the national share of votes. The departamento with the highest poverty incidence is Chocó.

<figure>
	<a href="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/spyder_plot.jpg"><img src="https://raw.githubusercontent.com/Felipe1990/personalblog/master/assets/img/post_2/spyder_plot.jpg" width="400" height="300" align="middle"></a>
</figure>

What I found interesting was that for the departamentos with the highest poverty incidecen the share of votes for Petro was higher than for those with the lowest poverty incidence (aside from Valle and Atlantico). It's interting that the data shows the expectations to be true.

Duque's figures would be redundant in this plot, since they would have the exactly opposite behaviour.

[^5]: [DANE figures for 2017. Monetary poverty incidence.](https://www.dane.gov.co/index.php/estadisticas-por-tema/pobreza-y-condiciones-de-vida/pobreza-y-desigualdad/pobreza-monetaria-y-multidimensional-en-colombia-2017#pobreza-monetaria-por-departamentos-2017)

Any comment is welcome (on the graphs, text, grammar, further insights, etc.)

Special thanks to...



