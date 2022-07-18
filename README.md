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

**Left Censoring**: This happens when the birth event wasn’t observed. Another concept known as Length-Biased Sampling should also be mentioned here. This type of sampling occurs when the goal of the study is to perform analysis on the people/subjects who already experienced the event and we wish to see whether they will experience it again. The lifelines package has support for left-censored datasets by adding the keyword left_censoring=True. Note that by default, it is set to False

**Interval Censoring**: This happens when the follow-up period, i.e time between observation, is not continuous. This can be weekly, monthly, quarterly, etc.

![17 07 2022_19 45 04_REC](https://user-images.githubusercontent.com/99672298/179472579-4c62bf3b-461f-4a5f-b538-50e4776ba29c.png)

**Left Truncation**: It is referred to as late entry. The subjects may have experienced the event of interest before entering the study. There is an argument named ‘entry’ that specifies the duration between birth and entering the study. If we fill in the truncated region then it will make us overconfident about what occurs in the early period after diagnosis. That’s why we truncate them

![17 07 2022_19 51 53_REC](https://user-images.githubusercontent.com/99672298/179472604-07915399-483e-4aa8-8e88-bec158925ce5.png)
