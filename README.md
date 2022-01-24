# Evaluating-UN-Peacekeeping-Operations

As part of the lecture "Multivariate Analyses" at the University of Mannheim, I conducted an analysis and evaluation of peacekeeping operations by the United Nations in African civil wars. This was done using R and RStudio.

Topic Introduction: 
While UN Peacekeeping Missions have originally been intended to support post-conflict peace processes, in the last two decades such missions are also commonly deployed to states with ongoing armed conflict. Such interventions into active conflict are considered to reduce the hostility between conflicting parties in order to diminish battlefield violence. The following analysis should test the hypothesis that the effect of the UN troops size on reducing the battle-related fatalities is stronger once a ceasefire agreement is in place. 
The given data set contains information about civil conflicts and the corresponding peacekeeping operations of the UN in Africa from 1989 to 2011. The dependent variable is the number of battlefield deaths caused by a governmentrebel group dyad in a specific month, including civilians.

Rough approach:
1) Data preprocessing
2) Exploratory data analysis
3) Model building with different attributes using the Negative Binomial Model
4) Robustness Checks
5) Create different scenarios to compute the expected number of battle-related fatalities and a 95% confidence interval
6) Data Visualization

Conclusion:
In conclusion, it was shown that the effect of the one month lagged UN troops size on reducing the battle-related fatalities is stronger once a ceasefire agreement is in place. This effect is shown to be robust among different models. Further interesting correlations such as the high effect of biased interventions, low efficiency of police troops compared to military troops or high number of deaths in countries with more population have been depicted. Therefore, despite the many criticisms, UN operations often are effective in reducing the number of fatalities. Obviously, there are other properties such as the timely development of peace after the UN intervention which can be analyzed in further steps. Additionally, instead  of using the Negative Binomial one could consider a Quasi-Poisson model which is very similar but can result in very different estimates.
