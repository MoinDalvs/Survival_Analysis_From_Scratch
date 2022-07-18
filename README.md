# Survival_Analysis_From_Scratch
An introduction to the concepts of Survival Analysis and its implementation in lifelines package for Python.

Survival Analysis is used to estimate the lifespan of a particular population under study. It is also called ‘Time to Event’ Analysis as the goal is to estimate the time for an individual or a group of individuals to experience an event of interest.In many medical studies, time to death is the event of interest. However, in cancer, another important measure is the time between response to treatment and recurrence or relapse-free survival time (also called disease-free survival time). Survival Analysis was originally developed and used by Medical Researchers and Data Analysts to measure the lifetimes of a certain population[1]. But, over the years, it has been used in various other applications such as:
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
https://miro.medium.com/max/582/1*2Dia2xF_DVMccQh8saPYFg.png\
One thing to point out here is that the Hazard is not a probability. This is because, even though we have the probability in the numerator, but the ΔT in the denominator could result in a value which is greater than one.

![17 07 2022_13 00 38_REC](https://user-images.githubusercontent.com/99672298/179527859-39c9a2d4-8f13-4de9-868b-f553500930fe.png)
![17 07 2022_13 10 55_REC](https://user-images.githubusercontent.com/99672298/179527877-33467681-139e-4795-aff1-d307d1a598cf.png)

## KAPLAN–MEIER SURVIVAL ESTIMATE
The survival probability can be estimated nonparametrically from observed survival times, both censored and uncensored, using the KM (or product-limit) method (Kaplan and Meier, 1958).Kaplan-Meier Estimate is used to measure the fraction of subjects who survived for a certain amount of survival time t under the same circumstances[2]. It is used to give an average view of the population. This method is also called the product limit. It allows a table called, life table, and a graph, called survival curve, to be produced for a better view of the population at risk. Survival Time is defined as the time starting from a predefined point to the occurrence of the event of interest. The Kaplan-Meier Survival Curve is the probability of surviving in a given length of time where time is considered in small intervals. For survival Analysis using Kaplan-Meier Estimate, there are three assumptions:

+ Subjects that are censored have the same survival prospects as those who continue to be followed.
+ Survival probability is the same all the subjects, irrespective of when they are recruited in the study.
![image](https://user-images.githubusercontent.com/99672298/179541258-992196da-5e59-4d90-bd50-7c2446f75aea.png)\
More formally,
+ the probability of being alive at time tj, 
+ S(tj), is calculated from S(tj−1) the probability of being alive at tj−1,
+ nj the number of patients alive just before tj,
+ and dj the number of events at tj
The survival probability at any particular time is calculated as the number of subjects surviving divided by the number of people at risk. The censored subjects are not counted in the denominator. The equation is given as follows:
![image](https://user-images.githubusercontent.com/99672298/179541548-923f61b6-e95f-4df9-8f36-a434e6eb6a17.png)\
Here, ni represents the number of subjects at risk prior to time t. di represents the number of the event of interest at time t.
Confidence intervals for the survival probability can also be calculated. The KM survival curve, a plot of the KM survival probability against time, provides a useful summary of the data that can be used to estimate measures such as median survival time. The large skew encountered in the distribution of most survival data is the reason that the mean is not often used.
