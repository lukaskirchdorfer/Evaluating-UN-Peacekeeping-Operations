---
title: "Evaluating UN Peacekeeping Operations"
author: "Lukas Kirchdorfer"
date: "Dezember 10, 2021"
output:
  pdf_document:
    template: template.tex
    latex_engine: xelatex
    toc: yes
    toc_depth: 2
    keep_tex: yes
    fig_width: 7
    fig_height: 6
    fig_caption: true
    keep_md: yes
  html_document:
    toc: yes
    toc_depth: '2'
    df_print: paged
    keep_md: yes
bibliography: bibliography.bib
csl-hanging-indent: yes
fontsize: 12pt 
linestretch: 2 # adjust for line spacing 
geometry: left=2.5cm,right=2.5cm,top=2.5cm,bottom=2.5cm
classoption:
- a4paper
- oneside
lang: en-EN
numbersections: yes
csquotes: yes
type: Data Essay for Course
course: Quantitative Methods in Political Science
subtitle: ''
address: ''
email: jdoe@mail.uni-mannheim.de
phone: ''
examiner: Prof. Thomas Gschwend, Ph.D.
chair: ''
mp: 0.55
ID: ''
wordcount: '*Wordcount excluding References: 1471*' 
csl: https://raw.githubusercontent.com/citation-style-language/styles/master/american-political-science-association.csl
editor_options:
  markdown:
    wrap: sentence
---





# Introduction

While UN Peacekeeping Missions have originally been intended to support post-conflict peace processes, in the last two decades such missions are also commonly deployed to states with ongoing armed conflict.
Such interventions into active conflict are intended to reduce the hostility between conflicting parties in order to reduce battlefield violence.
The following analysis should test whether increasing number of UN troops are associated with reduced battle-related fatalities with respect to the parties' willingness to agree to a ceasefire.

# Exploratory Data Analysis

The given data set contains information about civil conflicts and the corresponding peacekeeping operations of the UN in Africa from 1989 to 2011.
The dependent variable is the number of battlefield deaths caused by a government-rebel group dyad in a specific month, including civilians.
I include civilians as collateral damage because these fatalities are directly related to the civil wars and therefore should be taken into account as battle-related fatalities.
The average number of battle-related deaths per month is 13 while the median is 0 and the maximum is 9,793.
Therefore, the standard deviation is pretty high at 178.
Two personnel related independent variables I include in the model are given by the number of UN military troops in thousands and the number of UN police unites in thousands.
To consider timely delay of armed influence, the personnel numbers are lagged one month.
These personnel counts heavily depend on the combat which can be seen in the average and maximal values.
E.g., the average number of UN troops is 1,253 while the maximum number is 29,209.
The range in the number of police units is also large.
Besides these, another independent variable is the willingness to agree to a ceasefire which is encoded as 1 if there exists such an agreement in a given month, and as 0 if otherwise.
In 12,452 months there is not such an agreement while there is one on 9,066 months.
This variable is important because there might be a systematic difference between the effectiveness of UN interventions in combats where the parties are willing to stop fighting and combats where parties are not willing to lay down guns, as the hypothesis introduced in the first sections states.
Furthermore, the number of rebel groups involved in the conflict as well as the rebel strength compared to the government, ordinal encoded from 1 (rebels are much weaker than government) to 5 (rebels are much stronger than government).
The average rebel strength is 2.
To prevent spurious correlations of the effectiveness of the UN, I include biased intervention which accounts for additional states that might be involved in the war either in support of the government or the rebels.
Coded as 1 if more states intervened, 0 otherwise.
The logged population is also included as [@Fearon_and_Laitin_2003] suggest that the population has a positive impact on the likelihood of wars and deaths.
The logarithm ensures the scaling of the variable.
Last, I would include the one month lagged dependent variable as this is very likely to be correlated with the battle-related fatalities one month later.















# Model Development and Evaluation

The given dependent variable is a count of discrete values.
Therefore, a count model can be used to model the correlation.
As already shown, the standard deviation of the dependent variable is a lot higher than the mean, i.e. the data is overdispersed which leads to biased standard errors towards 0.
Therefore, instead of taking a Poisson distribution, I choose the negative binomial model, i.e.
I assume that the number of battlefield deaths caused by a government-rebel group dyad in a specific month including civilians follows a negative binomial distribution.

















```r
#first run: 
#stargazer(poisson_, neg_bin_1, neg_bin_3, type="text") # see order of variables and then rename them

stargazer(
  list(neg_bin_1, neg_bin_3, neg_bin_5, model_outlier),
  out = "table_lab.tex",
  title = "Regression Results",
  notes = "Model 4 exlcudes outliers",
  intercept.bottom = TRUE,
   covariate.labels = c(
     "UN Troops in thousand t-1",
     "UN Police in thousand t-1",
     "Ceasefire",
     "Strength of \\\\Rebel Group",
     "Number of Rebel Groups",
     "Battle-related \\\\fatalities t-1",
     "Population ln",
     "Biased Intervention",
     "Ceasefire x Battle-related \\\\fatalities t-1",
     "Constant"
   ),
  dep.var.labels = c("Battle-related fatalities in a month, incl. civilians"),
  table.placement = "H", # latex output, keep the figure at exactly Here in text
  type = "text" #stargazer_opt
)
```

```
## 
## Regression Results
## =====================================================================================================
##                                                         Dependent variable:                          
##                                ----------------------------------------------------------------------
##                                        Battle-related fatalities in a month, incl. civilians         
##                                       (1)               (2)               (3)              (4)       
## -----------------------------------------------------------------------------------------------------
## UN Troops in thousand t-1          -0.125***         -0.082***         -0.079***        -0.057***    
##                                     (0.020)           (0.014)           (0.014)          (0.013)     
##                                                                                                      
## UN Police in thousand t-1          0.410***                                                          
##                                     (0.150)                                                          
##                                                                                                      
## Ceasefire                          -0.476***         -0.663***         -1.362***        -1.352***    
##                                     (0.126)           (0.122)           (0.108)          (0.114)     
##                                                                                                      
## Strength of Group                   -0.003                                                           
##                                     (0.077)                                                          
##                                                                                                      
## Number of Rebel Groups             -0.108***         -0.092***                          -0.111***    
##                                     (0.016)           (0.015)                            (0.014)     
##                                                                                                      
## Battle-related t-1                 0.015***          0.015***          0.011***          0.025***    
##                                    (0.0003)          (0.0003)          (0.0003)          (0.0003)    
##                                                                                                      
## Population ln                      0.339***          0.344***                            0.277***    
##                                     (0.054)           (0.050)                            (0.047)     
##                                                                                                      
## Biased Intervention                2.354***          2.335***                            2.383***    
##                                     (0.280)           (0.278)                            (0.261)     
##                                                                                                      
## Ceasefire x Battle-related t-1                                         0.032***                      
##                                                                         (0.001)                      
##                                                                                                      
## Constant                            -1.139*          -1.202**          2.167***          -0.966**    
##                                     (0.593)           (0.482)           (0.069)          (0.453)     
##                                                                                                      
## -----------------------------------------------------------------------------------------------------
## Observations                        21,367            21,367            21,367            21,338     
## Log Likelihood                    -17,201.980       -17,198.870       -17,272.870      -16,650.110   
## theta                          0.018*** (0.0005) 0.018*** (0.0005) 0.017*** (0.0004) 0.021*** (0.001)
## Akaike Inf. Crit.                 34,421.950        34,411.730        34,555.740        33,314.220   
## =====================================================================================================
## Note:                                                                     *p<0.1; **p<0.05; ***p<0.01
##                                                                             Model 4 exlcudes outliers
```


# ```{r, header=False}
# # table
# 
# # first run: 
# stargazer(poisson_, neg_bin_1, neg_bin_3, neg_bin_4, neg_bin_5, type="text") # see order of variables and then rename them
# 
# stargazer(
#   list(poisson_, neg_bin_1),
#   out = "table_lab.tex",
#   title = "Regression Results",
#   notes = "Excluding observation Rwanda 1994",
#   intercept.bottom = TRUE,
#    covariate.labels = c(
#      "UN Troops t-1",
#      "UN Police t-1",
#      "Ceasefire",
#      "Strength of Rebel Group",
#      "Number of Rebel Groups",
#      "Battle-related fatalities t-1",
#      "Population ln",
#      "Biased Intervention",
#      "Ceasefire x Battle-related fatalities t-1",
#      "Constant"
#    ),
#   dep.var.labels = c("Battle-related fatalities in the government and rebel groups in a month, incl. civilians"),
#   table.placement = "H", # latex output, keep the figure at exactly Here in text
#   type = stargazer_opt
# )
# ```
# 
# ```{r}
# # Quantities of Interest
# 
# # 1. Get coefficients from regression
# nsim <- 1000
# gamma_hat_1 <- coef(neg_bin_)
# # 2. Get the variance-covariance matrix
# V_hat_1 <- vcov(neg_bin_)
# # 3. Set up a multivariate normal distribution
# S_1 <- mvrnorm(nsim, gamma_hat_1, V_hat_1)
# ```
# 
# ```{r}
# names(gamma_hat_1)
# ```
# 
# ```{r}
# prior_troops <- round(seq(
#   min(neg_bin_$model$troop_lag1000),
#   quantile(neg_bin_$model$troop_lag1000, 0.95),
#   length.out = 1
# ))
# 
# # scenario 1: no ceasefire agreement
# scenario1 <- cbind(
#   1, # intercept
#   median(prior_troops, na.rm = TRUE),
#   median(df_clean$police_lag1000, na.rm = TRUE),
#   0, # no ceasefire
#   median(df_clean$rebel_strength, na.rm = TRUE),
#   median(df_clean$n_rebel_groups, na.rm = TRUE),
#   median(df_clean$brf_grc_lag, na.rm = TRUE),
#   median(df_clean$pop_ln, na.rm = TRUE),
#   median(df_clean$biased_intervention, na.rm = TRUE)
# )
# colnames(scenario1) <- names(gamma_hat_1)
# 
# # scenario 2: ceasefire agreement
# scenario2 <- scenario1
# scenario2[which(colnames(scenario2) == "ceasefire")] <- 1
# 
# head(scenario1)
# head(scenario2)
# ```
# 
# ```{r}
# Xbeta1_1 <- S_1 %*% t(scenario1)
# Xbeta2_1 <- S_1 %*% t(scenario2)
# ```
# 
# ```{r}
# # get expected values for lambda
# lambda1_1 <- exp(Xbeta1_1)
# lambda2_1 <- exp(Xbeta2_1)
# 
# # plug lambda and theta into neg binomial distribution
# theta_1 <- neg_bin_$theta
# 
# exp_no_ceasefire <-
#   apply(lambda1_1, c(1, 2), function(x) {
#     mean(rnbinom(10000, size = theta_1, mu = x))
#   })
# 
# exp_ceasefire <-
#   apply(lambda2_1, c(1, 2), function(x) {
#     mean(rnbinom(10000, size = theta_1, mu = x))
#   })
# ```
# 
# ```{r}
# quants_no_ceasefire <- t(apply(exp_no_ceasefire, 2, quantile, c(0.025, 0.5, 0.975)))
# quants_ceasefire <- t(apply(exp_ceasefire, 2, quantile, c(0.025, 0.5, 0.975)))
# colnames(quants_no_ceasefire)
# colnames(quants_ceasefire)
# ```
# 
# ```{r}
# # plot the scenarios
# # polygon plot
# plot(
#   prior_troops,
#   quants_no_ceasefire[, "50%"],
#   type = "n",
#   ylim = c(0, 9000),
#   xlim = c(0, 30),
#   ylab = "Number of Battle deaths",
#   xlab = "UN Troops",
#   bty = "n",
#   main = "Expected Deaths in Civil War Situations"
# )
# 
# polygon(
#   c(rev(prior_troops), prior_troops),
#   c(rev(quants_no_ceasefire[, "97.5%"]), quants_no_ceasefire[, "2.5%"]),
#   col = viridis(3, 0.2)[3],
#   border = NA
# )
# 
# polygon(
#   c(rev(prior_troops), prior_troops),
#   c(rev(quants_ceasefire[, "97.5%"]), quants_ceasefire[, "2.5%"]),
#   col = viridis(3, 0.2)[2],
#   border = NA
# )
# 
# 
# lines(prior_troops, quants_no_ceasefire[, 2], lwd = 2, lty = "dashed", col = viridis(3, 0.5)[3])
# lines(prior_troops, quants_no_ceasefire[, 1], lwd = 0.5, lty = "dashed", col = viridis(3, 0.5)[3])
# lines(prior_troops, quants_no_ceasefire[, 3], lwd = 0.5, lty = "dashed", col = viridis(3, 0.5)[3])
# 
# lines(prior_troops, quants_ceasefire[, 2], lwd = 2, lty = "dotted", col = viridis(3, 0.5)[2])
# lines(prior_troops, quants_ceasefire[, 1], lwd = 0.5, lty = "dashed", col = viridis(3, 0.5)[2])
# lines(prior_troops, quants_ceasefire[, 3], lwd = 0.5, lty = "dashed", col = viridis(3, 0.5)[2])
# 
# 
# 
# # Add a "histogram" of actual X1-values.
# axis(
#   1,
#   at = df_clean$troop_lag1000,
#   col.ticks = "gray30",
#   labels = FALSE,
#   tck = 0.02
# )
# 
# legend(
#   "topleft",
#   legend = c(
#     "Median & 95% CI:",
#     "No Ceasefire",
#     "Ceasefire"
#   ),
#   col = c(
#     "white",
#     viridis(3, 0.5)
#   ),
#   lty = c(NA, "solid", "dotted", "dashed"),
#   lwd = c(NA, 2, 2, 2),
#   # pch = c(NA, 15, NA, 15, NA, 15),
#   pt.cex = 2,
#   bty = "n"
# )
# ```
# 
# ```{r}
# # First Difference:
# FD <- exp_no_ceasefire - exp_ceasefire
# quants_FD <- t(apply(FD, 2, quantile, c(0.025, 0.5, 0.975)))
# ```


Please see the documentation of [RMarkdown](http://rmarkdown.rstudio.com/) for more details on how to write RMarkdown documents.

## My Subsection Header 2

"Lorem ipsum" dolor sit amet, consectetur adipiscing elit.
Proin mollis dolor vitae tristique eleifend.
Quisque non ipsum sit amet velit malesuada consectetur.
Praesent vel facilisis leo.
Sed facilisis varius orci, ut aliquam lorem malesuada in.
Morbi nec purus at nisi fringilla varius non ut dui.
Pellentesque bibendum sapien velit.
Nulla purus justo, congue eget enim a, elementum sollicitudin eros.
Cras porta augue ligula, vel adipiscing odio ullamcorper eu.
In tincidunt nisi sit amet tincidunt tincidunt.
Maecenas elementum neque eget dolor [egestas fringilla](http://example.com):

> Nullam eget dapibus quam, sit amet sagittis magna.
> Nam tincidunt, orci ac imperdiet ultricies, neque metus ultrices quam, id gravida augue lacus ac leo.

Vestibulum id sodales lectus, sed scelerisque quam.
Nullam auctor mi et feugiat commodo.
Duis interdum imperdiet nulla, vitae bibendum eros placerat non.
Cras ornare, risus in faucibus malesuada, libero sem fringilla quam, ut luctus enim sapien eget dolor.
Vestibulum id sodales lectus, sed scelerisque quam.
Nullam auctor mi et feugiat commodo.
Duis interdum imperdiet nulla, vitae bibendum eros placerat non.
Cras ornare, risus in faucibus malesuada, libero sem fringilla quam, ut luctus enim sapien eget dolor.
- Footnotes are supported[^1].

[^1]: Here is how you add a footnote, and include a citation there as well @imai_quantitative_2017

# R-Code

You can also add the R code and a plot:

\begin{figure}[h]

{\centering \includegraphics[width=\textwidth]{figs/mtcars-1} 

}

\caption[My Title]{My Title}\label{fig:mtcars}
\vspace{0.5cm} \footnotesize{\textit{Notes: }This is how you can add notes for a plot in PDF. In academic context, it is quite common to put information like model used for obtaining quantities of interest and CI levels in notes. In general, figures should stand alone and require no reading of the text for comprehension.} \end{figure}

# Citation

You can also embed plots, for example: Figure \ref{fig:mtcars}.
Markdown will create numbering automatically, and you can reference figures and tables with their respective labels that you set up when making figures.
A reference to the figure we included will look like this `\ref{fig:mtcars}` and will print out only the number of the object (you have to add figure/title on your own).

Use this format for citation: `[@bibtexkey]`.
Put all the bibliography data in one bibliography file.

\newpage

# References {.unnumbered}

\singlespacing

::: {#refs}
:::

\newpage

# Appendix {.unnumbered}

<!-- Line below depicts the content that should not be counted in the wordcount -->

<!---TC:ignore--->

<!-- The lines below adjust page numbering and figure/table numbering to add A before.  -->

```{=tex}
\renewcommand*{\thepage}{A\arabic{page}}
\renewcommand*{\thesubsection}{\Alph{subsection}.}
\renewcommand*{\thesubsubsection}{\alph{subsubsection}.}
\renewcommand\thefigure{A\arabic{figure}}   
\renewcommand\thetable{A\arabic{table}}  
\setcounter{figure}{0}
\setcounter{table}{0}
\setcounter{page}{1}
```
## Same Plot

Vestibulum id sodales lectus, sed scelerisque quam.
Nullam auctor mi et feugiat commodo.
Duis interdum imperdiet nulla, vitae bibendum eros placerat non.
Cras ornare, risus in faucibus malesuada, libero sem fringilla quam, ut luctus enim sapien eget dolor.
Vestibulum id sodales lectus, sed scelerisque quam.
Nullam auctor mi et feugiat commodo.
Duis interdum imperdiet nulla, vitae bibendum eros placerat non.
Cras ornare, risus in faucibus malesuada, libero sem fringilla quam, ut luctus enim sapien eget dolor.
Vestibulum id sodales lectus, sed scelerisque quam.
Nullam auctor mi et feugiat commodo.
Duis interdum imperdiet nulla, vitae bibendum eros placerat non.
Cras ornare, risus in faucibus malesuada, libero sem fringilla quam, ut luctus enim sapien eget dolor.

\begin{figure}[h]

{\centering \includegraphics[width=\textwidth]{figs/mtcars1-1} 

}

\caption[My Title]{My Title}\label{fig:mtcars1}
\end{figure}

## Same Plot Yet Again

Vestibulum id sodales lectus, sed scelerisque quam.
Nullam auctor mi et feugiat commodo.
Duis interdum imperdiet nulla, vitae bibendum eros placerat non.
Cras ornare, risus in faucibus malesuada, libero sem fringilla quam, ut luctus enim sapien eget dolor.
Vestibulum id sodales lectus, sed scelerisque quam.
Nullam auctor mi et feugiat commodo.
Duis interdum imperdiet nulla, vitae bibendum eros placerat non.
Cras ornare, risus in faucibus malesuada, libero sem fringilla quam, ut luctus enim sapien eget dolor.
Vestibulum id sodales lectus, sed scelerisque quam.

\begin{figure}[h]

{\centering \includegraphics[width=\textwidth]{figs/mtcars2-1} 

}

\caption[My Title]{My Title}\label{fig:mtcars2}
\vspace{0.5cm} \footnotesize{\textit{Notes: }This is how you can add notes for a plot in PDF. In academic context, it is quite common to put information like model used for obtaining quantities of interest and CI levels in notes. In general, figures should stand alone and require no reading of the text for comprehension.} \end{figure}

\clearpage

# Statutory Declaration {.unnumbered}

Hiermit versichere ich, dass diese Arbeit von mir persönlich verfasst ist und dass ich keinerlei fremde Hilfe in Anspruch genommen habe.
Ebenso versichere ich, dass diese Arbeit oder Teile daraus weder von mir selbst noch von anderen als Leistungsnachweise andernorts eingereicht wurden.
Wörtliche oder sinngemäße Übernahmen aus anderen Schriften und Veröffentlichungen in gedruckter oder elektronischer Form sind gekennzeichnet.
Sämtliche Sekundärliteratur und sonstige Quellen sind nachgewiesen und in der Bibliographie aufgeführt.
Das Gleiche gilt für graphische Darstellungen und Bilder sowie für alle Internet-Quellen.
Ich bin ferner damit einverstanden, dass meine Arbeit zum Zwecke eines Plagiatsabgleichs in elektronischer Form anonymisiert versendet und gespeichert werden kann.
Mir ist bekannt, dass von der Korrektur der Arbeit abgesehen werden kann, wenn die Erklärung nicht erteilt wird.

```{=tex}
\SignatureAndDate{}
\renewcommand*{\thepage}{ }
```
\noindent I hereby declare that the paper presented is my own work and that I have not called upon the help of a third party.
In addition, I affirm that neither I nor anybody else has submitted this paper or parts of it to obtain credits elsewhere before.
I have clearly marked and acknowledged all quotations or references that have been taken from the works of other.
All secondary literature and other sources are marked and listed in the bibliography.
The same applies to all charts, diagrams and illustrations as well as to all Internet sources.
Moreover, I consent to my paper being electronically stores and sent anonymously in order to be checked for plagiarism.
I am aware that the paper cannot be evaluated and may be graded "failed" ("nicht ausreichend") if the declaration is not made.

\SignatureAndDateEng{}

<!-- Line below depicts the content that should not be counted in the wordcount -->

<!---TC:ignore--->
