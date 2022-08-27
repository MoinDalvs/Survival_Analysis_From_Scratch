# Survival_Analysis_From_Scratch
An introduction to the concepts of Survival Analysis and its implementation in lifelines package for Python.

Survival Analysis is used to estimate the lifespan of a particular population under study. It is also called ‘Time to Event’ Analysis as the goal is to estimate the time for an individual or a group of individuals to experience an event of interest.In many medical studies, time to death is the event of interest. However, in cancer, another important measure is the time between response to treatment and recurrence or relapse-free survival time (also called disease-free survival time). Survival Analysis was originally developed and used by Medical Researchers and Data Analysts to measure the lifetimes of a certain population. But, over the years, it has been used in various other applications such as:
+ Predicting churning customers/employees
+ Cancer studies for patients survival time analyses,
+ Sociology for “event-history analysis”,
+ and in engineering for “failure-time analysis”.
![17 07 2022_12 50 34_REC](https://user-images.githubusercontent.com/99672298/179462411-f49bb640-2b73-4821-9bf9-00fdc8262692.png)

### In cancer studies, typical research questions are like:

+ What is the impact of certain clinical characteristics on patient’s survival
+ What is the probability that an individual survives 3 years?
+ Are there differences in survival between groups of patients?

## Data
In survival analysis, we do not need the exact starting points and ending points. All the observation do not always start at zero. A subject can enter at any time in the study. All the duration are relative. All the subjects are bought to a common starting point where the time t is zero (t = 0) and all subjects have the survival probabilities equal to one, i.e their chances of not experiencing the event of interest (death, churn, etc) is 100%.

![Screenshot 2022-07-17 193554](https://user-images.githubusercontent.com/99672298/179466638-a00cb748-d730-433a-8938-fbc640b01d3c.png)
![Screenshot 2022-07-17 193712](https://user-images.githubusercontent.com/99672298/179466647-0429e7c6-0b63-47c5-a977-4cd932d320ec.png)

There may arise situations where the volume of the data prevents it to be used completely in Survival Analysis. For such situations, Stratified Sampling may help. In Stratified Sampling, our goal is to have an equal or nearly equal amount of subjects from each group of subjects in the whole population. Each group is called a `Strata`. The whole population is stratified or divided into groups based on some characteristic. Now, in order to pick a certain number of subjects from each group, you can use `Simple Random Sampling`. 

## Censorship
The specific difficulties relating to survival analysis arise largely from the fact that only some individuals have experienced the event and, subsequently, survival times will be unknown for a subset of the study group. This phenomenon is called censoring and it may arise in the following ways: (a) a patient has not (yet) experienced the relevant outcome, such as relapse or death, by the time of the close of the study; (b) a patient is lost to follow-up during the study period; (c) a patient experiences a different event that makes further follow-up impossible another example, there will be customers who are still a member of the company, or employees still working for the company, or machines that are still functioning during the observation/study period. We do not know when they will experience the event of interest as of the time of the study. All we know that they haven’t experienced it yet. Their survival times are longer than their time in the study. Their survival times are thus, labelled as `‘Censored’`
Therefore, `Censorship allows you to measure lifetimes for the population who haven’t experienced the event of interest yet.`

The people/subjects who didn’t experience the event of interest need to be a part of the study as removing them completely would introduce bias towards the results of everyone in the study experiencing the event of interest. So, we cannot simply ignore those members and the only way to distinguish them from the ones who experienced the event of interest is to have a variable that indicates censorship or death (the event of interest).

There are different types of Censorship done in Survival Analysis as explained below. Note that Censoring must be independent of the future value of the hazard for that particular subject

**Right Censoring**: This happens when the subject enters at t=0 i.e at the start of the study and terminates before the event of interest occurs. This can be either not experiencing the event of interest during the study, i.e they lived longer than the duration of the study, or could not be a part of the study completely and left early without experiencing the event of interest, i.e they left and we could not study them any longer.

**Left Censoring**: This happens when the birth event wasn’t observed. Censoring can also occur if we observe the presence of a state or condition but do not know where it began. For example, consider a study investigating the time to recurrence of a cancer following surgical removal of the primary tumour. If the patients were examined 3 months after surgery to determine recurrence, then those who had a recurrence would have a survival time that was left censored because the actual time of recurrence occurred less than 3 months after surgery.
Another concept known as Length-Biased Sampling should also be mentioned here. This type of sampling occurs when the goal of the study is to perform analysis on the people/subjects who already experienced the event and we wish to see whether they will experience it again. The lifelines package has support for left-censored datasets by adding the keyword left_censoring=True. Note that by default, it is set to False

**Interval Censoring**: This happens when the follow-up period, i.e time between observation, is not continuous. If we consider the previous example and patients are also examined at 6 months, then those who are disease free at 3 months and lost to follow-up between 3 and 6 months are considered interval censored

![17 07 2022_19 45 04_REC](https://user-images.githubusercontent.com/99672298/179472579-4c62bf3b-461f-4a5f-b538-50e4776ba29c.png)

Survival Analysis was developed to mainly solve the problem of right-censoring, but methods for interval and left censored data are available (Hosmer and Lemeshow, 1999)

**Left Truncation**: It is referred to as late entry. The subjects may have experienced the event of interest before entering the study. There is an argument named ‘entry’ that specifies the duration between birth and entering the study. If we fill in the truncated region then it will make us overconfident about what occurs in the early period after diagnosis. That’s why we truncate them

![17 07 2022_19 51 53_REC](https://user-images.githubusercontent.com/99672298/179472604-07915399-483e-4aa8-8e88-bec158925ce5.png)

## SURVIVAL AND HAZARD
Survival data are generally described and modelled in terms of two related probabilities, namely survival and hazard. The survival probability (which is also called the survivor function) S(t) is the probability that an individual survives from the time origin (e.g. diagnosis of cancer) to a specified future time t.\
The Survival Function is given by,\
![image](https://user-images.githubusercontent.com/99672298/179503810-a58d9bb2-2b61-4eaf-a3c9-d5f263a6c86e.png)\
Survival Function defines the probability that the event of interest has not occurred at time t. It can also be interpreted as the probability of survival after time t . Here, T is the random lifetime taken from the population and it cannot be negative.

## Hazard Function
The Hazard Function also called the intensity function, is defined as the probability that the subject will experience an event of interest within a small time interval, provided that the individual has survived until the beginning of that intervalPut another way, it represents the instantaneous event rate for an individual who has already survived to time tIt can also be considered as the risk of experiencing the event of interest at time t. It is the number of subjects experiencing an event in the interval beginning at time t divided by the product of the number of subjects surviving at time t and interval widthSince the probability of a continuous random variable to equal a particular value is zero. That’s why we consider the probability of the event happening at a particular interval of time from T till (T + ΔT). Since our goal is to find the risk of an event and we don’t want the risk to get bigger as the time interval ΔT gets bigger. Thus, in order to adjust for that, we divide the equation by ΔT. This scales the equation by ΔT. The equation of the Hazard Rate is given as:\
![image](https://user-images.githubusercontent.com/99672298/179730557-4f49474c-8b3e-4d52-b633-468847bdd1bb.png)\
One thing to point out here is that the Hazard is not a probability. This is because, even though we have the probability in the numerator, but the ΔT in the denominator could result in a value which is greater than one.

![17 07 2022_13 00 38_REC](https://user-images.githubusercontent.com/99672298/179527859-39c9a2d4-8f13-4de9-868b-f553500930fe.png)
![17 07 2022_13 10 55_REC](https://user-images.githubusercontent.com/99672298/179527877-33467681-139e-4795-aff1-d307d1a598cf.png)

## KAPLAN–MEIER SURVIVAL ESTIMATE
The survival probability can be estimated nonparametrically from observed survival times, both censored and uncensored, using the KM (or product-limit) method (Kaplan and Meier, 1958).Kaplan-Meier Estimate is used to measure the fraction of subjects who survived for a certain amount of survival time t under the same circumstances. It is used to give an average view of the population. This method is also called the product limit. It allows a table called, life table, and a graph, called survival curve, to be produced for a better view of the population at risk. Survival Time is defined as the time starting from a predefined point to the occurrence of the event of interest. The Kaplan-Meier Survival Curve is the probability of surviving in a given length of time where time is considered in small intervals. For survival Analysis using Kaplan-Meier Estimate, there are three assumptions:
![17 07 2022_20 16 04_REC](https://user-images.githubusercontent.com/99672298/179729377-4836291e-43c5-4bce-9e15-12f50c2500fe.png)

+ Subjects that are censored have the same survival prospects as those who continue to be followed.
+ Survival probability is the same all the subjects, irrespective of when they are recruited in the study.
![image](https://user-images.githubusercontent.com/99672298/179541258-992196da-5e59-4d90-bd50-7c2446f75aea.png)\
Where,

+ S(ti−1) = the probability of being alive at ti−1
+ ni = the number of patients alive just before ti
+ di = the number of events at ti
+ t0 = 0, S(0) = 1
The estimated probability (S(t)) is a step function that changes value only at the time of each event. It’s also possible to compute confidence intervals for the survival probability.

The KM survival curve, a plot of the KM survival probability against time, provides a useful summary of the data that can be used to estimate measures such as median survival time.
The survival probability at any particular time is calculated as the number of subjects surviving divided by the number of people at risk. The censored subjects are not counted in the denominator. The equation is given as follows:
![image](https://user-images.githubusercontent.com/99672298/179541548-923f61b6-e95f-4df9-8f36-a434e6eb6a17.png)\
Here, ni represents the number of subjects at risk prior to time t. di represents the number of the event of interest at time t.
Confidence intervals for the survival probability can also be calculated. The KM survival curve, a plot of the KM survival probability against time, provides a useful summary of the data that can be used to estimate measures such as median survival time. The large skew encountered in the distribution of most survival data is the reason that the mean is not often used.
![Screenshot 2022-07-17 134251](https://user-images.githubusercontent.com/99672298/179729429-f83ceac1-a5b2-4bb1-8223-3d83d7105a02.png)\

## NONPARAMETRIC TESTS COMPARING SURVIVAL
Survival in two or more groups of patients can be compared using a nonparametric test. The logrank test (Peto et al, 1977) is the most widely used method of comparing two or more survival curves. The groups may be treatment arms or prognostic groups (e.g. FIGO stage). The method calculates at each event time, for each group, the number of events one would expect since the previous event if there were no difference between the groups. These values are then summed over all event times to give the total expected number of events in each group, say Ei for group i. The logrank test compares observed number of events, say Oi for treatment group i, to the expected number by calculating the test statistic\
![image](https://user-images.githubusercontent.com/99672298/179567699-d09db01b-52b7-446c-a3c3-546f7f44855c.png)\
This value is compared to a χ2 distribution with (g−1) degrees of freedom, where g is the number of groups. In this manner, a P-value may be computed to calculate the statistical significance of the differences between the complete survival curves.

If the groups are naturally ordered, a more appropriate test is to consider the possibility that there is a trend in survival across them, for example, age groups or stages of cancer. Calculating Oi and Ei for each group on the basis that survival may increase or decrease across the groups results in a more powerful test. For the new Oi and Ei, the test statistic for trend is compared with the χ2 distribution with one degree of freedom (Collett, 1994).

When only two groups are compared, the logrank test is testing the null hypothesis that the ratio of the hazard rates in the two groups is equal to 1. The hazard ratio (HR) is a measure of the relative survival experience in the two groups and may be estimated by\
![image](https://user-images.githubusercontent.com/99672298/179567760-a61f21c7-6ced-45ff-a914-e3025731fa83.png)\
An HR of 1 indicates no difference in survival. In practice, it is better to estimate HRs using a regression modelling technique, such as Cox regression.

 Nonetheless, the KM method has important limitations. First, it does not provide an effect estimate (i.e., a relative risk) or the related confidence interval to compare the survival in different patient groups in another word,the log-rank test is purely a significance test and cannot provide an estimate of the size of the difference between the groups and its related confidence interval.Second, it does not permit the adjustment of confounders (A confounding variable, also called a confounder or confounding factor, is a third variable in a study examining a potential cause-and-effect relationship. A confounding variable is related to both the supposed cause and the supposed effect of the study) in in etiological research or predictors in prognostic research (Prognostic research provides information essential for understanding and predicting future clinical outcomes in people with/in a specific health state).\

 Finally, the KM method requires data categorization, so calculation of the incremental increase (or decrease) in the relative risk of a given event associated with one unit (or any number of units) increase in the candidate risk factor is not possible.\
 These limitations can be approached by Cox regression analysis, in which the dependent variable is the incidence variable are risk factors or predictors that the investigators use to explain the or predict the study endpoint.\
  Cox regression is used in survival time analysis to determine the influence of different variables on survival time, variables such as age, clinical history, exposure to certain factors and time.\
  Cox regression is also kwnon as Cox proportional hazards model.\
  In Survival Analysis, after a disease has been detected we are not only interested in the survival time but also want to know whether survival time depends on one or more factors called predictors.\
  For some situations with a single factor wiht only two values Log Rank test is used for example to test the difference between the survival time when two different types of drugs are given.\
  The Cox Proportional Hazards Regression Analysis Model was introduced by Cox and it takes into account the effect of several variables at a time and examines the relationship of the survival distribution to these variables. It is similar to Multiple Regression Analysis, but the difference is that the depended variable is the Hazard Function at a given time t. It is based on very small intervals of time, called time-clicks, which contains at most one event of interest. It is a semi-parametric approach for the estimation of weights in a Proportional Hazard Model.\
  The formula for the Cox Proportional Hazards Regression Model is given as follows. The model works such that the log-hazard of an individual subject is a linear function of their static covariates and a population-level baseline hazard function that changes over time.\
  ![image](https://user-images.githubusercontent.com/99672298/179714514-2f1cf4f1-134b-42fa-a6dd-3bf167d7c68a.png)\
  β0(t) is the baseline hazard function and it is defined as the probability of experiencing the event of interest when all other covariates equal zero. And It is the only time-dependent component in the model. The model makes no assumption about the baseline hazard function and assumes a parametric form for the effect of the covariates on the hazard.\
  The sign of the regression coefficients, βi, plays a role in the hazard of a subject. A change in these regression coefficients or covariates will either increase or decrease the baseline hazard. A positive sign for βi means that the risk of an event is higher, and thus the prognosis for the event of interest for that particular subject is higher. Similarly, a negative sign means that the risk of the event is lower.\
Cox Model can handle right-censored data but cannot handle left-censored or interval-censored data directly\
## Aalen’s Additive Model
Like the Cox model, this model is also a regression model but unlike the Cox model, it defines the hazard rate as an additive instead of a multiplicative linear model. The hazard is defined as:

![image](https://user-images.githubusercontent.com/99672298/179730180-d45797cb-add2-4312-91b0-5ebda0a511ed.png)

During estimation, the linear regression is computed at each step. The regression can become unstable due to small sample sizes or high colinearity in the dataset.
This is a parametric model, which means that it has a functional form with parameters that we are fitting the data to. Parametric models allow us to extend the survival function, hazard function, or the cumulative hazard function past our maximum observed duration. This concept is called Extrapolation

## Author

<table>
<tr>
<td>
     <img src="https://avatars.githubusercontent.com/u/99672298?v=4" width="180"/>
     
     moindalvs@gmail.com

<p align="center">
<a href = "https://github.com/MoinDalvs"><img src = "http://www.iconninja.com/files/241/825/211/round-collaboration-social-github-code-circle-network-icon.svg" width="36" height = "36"/></a>
<a href = "https://twitter.com/DalvsHubot"><img src = "https://www.shareicon.net/download/2016/07/06/107115_media.svg" width="36" height="36"/></a>
<a href = "https://www.linkedin.com/in/moin-dalvi-277b0214a//"><img src = "http://www.iconninja.com/files/863/607/751/network-linkedin-social-connection-circular-circle-media-icon.svg" width="36" height="36"/></a>
</p>
</td>
</tr> 
  </table>

!!!Thank You For Reading!!!
