# Chicago Freshman OnTrack: Fact or Fiction? 
## Definitions 
* **Freshman on Track (FOT)**: A first-time ninth grader is considered to be "On-Track" if they accumulated at least 5 course credits during their first year of high school, failed no more than 0.5 course credits in a core course (English, Math, Social Studies or Science) and did not leave the school as a dropout.
* **Attendance over time**: average daily student attendance rate is the percentage of the total number of days in which students during the school year were marked present at a school, divided by the total number of days that those students were expected to be in attendance
* **Cohort**:  a group of ninth graders who enroll in high school for the first time during a school year. The cohort dropout rates are the percentages of a cohort who drop out of school within the 5-year and 4-year time spans.
* **College Enrollment Rate**: measures the percentage of students graduating from a school who in the previous year who enrolled in a 2-year or 4-year college in the fall after graduation from high school
* **College Persistence Rate**: measures the percentage of students counted in the college enrollment rate that remained enrolled in college the following fall

## Overview
Using publicly available data form Chicago Public Schools, we set out to analyze the claim from UIC and the To&Through program that how a student performs during their freshman year is the most accurate predictor of whether or not a student will  graduate. Using predictive modeling, clustering, and PCA analysis, we discovered that this claim is, by and large, true. However, issues with data availability, aggregation, and consistency prevented us from exploring our initial question: can we predict if a student will be “On Track” in their freshman year based on 8th grade testing scores and attendance data? 

## Project Summary 
A [joint report](https://toandthrough.uchicago.edu/sites/default/files/uploads/documents/Data%20Insights%202020%20v5_0.pdf) published by the University of Illinois (UIC), UChicago Consortium on School Research, and the To&Through Project preports that how a 9th grade high school student performs during their freshman year is the most reliable way to predict future success for that student. In other words, whether or not a high school freshman meets the criteria to be considered "OnTrack" is `"more predictive of a students odds of graduating"` than all other factors combined--- including `"gender, race, socioeconimic status, 8th grade test scores, mobility prior to High School, and Overage for grade."`The report also claimed that freshman who are OnTrack graduate at a rate of `87%` compared to "off-track" students who graduate at a rate of only `30%`.

At face value, these findings presented an interesting research opportunity;
initially, we set out to build a predictive model that could examine an 8th grade cohort's RIT test scores. We then planned to train this model to predict whether or not the results of these scores could be used to predict with at least 80% accuracy whether or not these cohorts would go on to be "OnTrack" as they entered their freshman year of high school. However, as we began developing our models and digging deeper into our collected data, we uncovered several issues:
  1. There is no reliable way to track any given cohort from 8th to 9th grade with the data we have access to, 
  2. Mobility of students before and during middle and high school years makes it difficult to determine with any certainty which cohorts contain transfer students and how this may impact the accuracy of our model

### Changing Directions 
Our first and perhaps most insurmountable issue, given the time constraints under which we were opperating, was finding a reliable way to track the 8th grade cohorts into their 9th grade year. 
* What did we not take into account? 
* What made this tracking improbable? 

Upon closer examination of the [joint report](https://toandthrough.uchicago.edu/sites/default/files/uploads/documents/Data%20Insights%202020%20v5_0.pdf) and the discovery of the original FOT report--- ["The Use of Ninth-Grade Early Warning Indicators to Improve Chicago Schools"](https://nuvirtdatapt1-tlp6224.slack.com/files/U04EKRZ5A77/F05BS2AEJUC/jesparon-track.pdf) in the Journal of Education for Students Placed at Risk (JESPAR), we concluded that, due to an apparent lack of peer-reviewed data and replicatable results, using FOT as our guiding metric for predictive success may not produce the outcomes we set out to obtain. This realization, in tandem with the near-impossibility of reliably tracking cohorts across grades, led us to shift our focus. 

### Final Decisions 
After a brief discussion, we set out to answer the question: is FOT really the most accurate predictor of high school graducation? Our goal became to use the insights previously gleamed form our initial data gathering and normalization; this time, however, we took a look at the percentage of freshman considered OnTrack and the overall graduation rates of each school in our dataset. Then, using multiple predictive tests, compare our models to existing graducation and college enrollment/persistence data. 

## Findings 
### Logistic Regression Model

#### Model Accuracy 
* Mean Absolute Error (MAE): 8.281
* Mean Squared Error (MSE): 103.251
* Root Mean Squared Error (RMSE): 10.161
* R-squared: 0.292 Adjusted 
* R-squared: 0.255
* Cross validation scores: [ 0.021, 0.133, -1.101, 0.338, 0.118] 
* Mean cross validation score: -0.098

### PCA and Clustering 
We attempted to use PCA as a method for validating the findings from University of Chicago. To do this, we carried out a principal component analysis to find the strongest contributers to any clusters that might arise from our dataset. First performing a k-means and elbow analysis, we find an optimum number k=3 for our clusters. Looking specifically at the cohort of incoming freshmen in 2015 and the respective college persistence rates after 2019, we see our clusters are split fairly cleanly along the lines of high enrollment/high persistence, middle enrollment/middle persistence, and low enrollment/low persistence. There are, however, some outliers in the middle group that also have high persistence levels.

Our next step was to run PCA in Python to find out the main contributing components to these clusters for comparison against the University of Chicago's claims. Breaking our data down into 3 components, we obtain an explained_variance_ratio of 0.8980461428307921. We then set a cutoff value of 0.25 to display only the highest contributors from pca.components_. What we find is that the top contributors are simply the On-Track Rate for 2015-2019, as predicted.

### Linear Regression
Using linear regression to provide a visual summary of our findings- we found that there is a linear relationship between the overall rate of attendance and student performance, measured by the Freshman On-Track score (FOT). As expected, the FOT rose along with the Attendance Rate- up to a point.

There were several outliers and the correlation between the dependent variable (FOT) and the independent variable (Attendance) in this model appears to weaken past a certain percentage. So Attendance appears to be a driver for FOT up to a certain point. The reason for this change in correlation may be due to several known or unknown factors, but there are also concerns that inconsistency in the raw data may have had some effect on the findings as well. 

We also found that not all schools participated or provided student FOT data. Also, there were a number of schools that were not open at either the begining or the end of the observation period. As such, this limited the number of schools that had consistent data available for the entire observation period.

The overall results however, appear to support the findings of the UIC study- school attendance has a positive correlation to student performance. As the attendance rate increases, so to does the performance, measured by the Freshman On-Track score. The relationship appears to be stronger and more pronounced where the attendance rate nears 90%. Above the 90% range, we begin to see more outliers and exceptions  and the corelation appears to weaken.

In addition, it may be likely that linear regression might not be the best analytical tool for analyizing this particular collection of data. After a certain point, a non-linear technique may be more appropriate for determining the relationship. 

## Limitations 
* Aggregate data 
* Time constraints 
* Tracking cohorts across grades 
* MESSY, messy data! 
* Unreplicated studies 
* Only used data from 2015-2019 due to a change in how FOT status was calculated pre-2015. Post-2019 data excluded due to pandemic. 
* Current data only shows information by school instead of by student
  * This is to avoid tracking specific students i.e. Male student of Hispanic descent transferred from (low-pop.) school A to (low-pop.) school B 
* How can we anonymize student data?

This project drew from publicly available data provided by Chicago Public Schools and the State of Illinois. While there was an abundance of public data about overall test scores and attendance, we found that some of the information was obscured or was intentially left qpaque to ensure student anonymity, and in some cases to provide some form of cover for lower performing schools. We also found that school closures and openings throughout the observation period, along with several exceptions to grade level and program size, very likely skewed the results and did not offer many viable options for detection and correction for these outliers. 

## Future Direcions 
### Anonymizing student data
* Publish data from large population schools where a reasonable degree of anonymity can be established within the student body
* Publish data after a certain period of time following graduation (ex. 5-10 years past)

Having individualized data will allow us to track progress by student instead of by school. With this data, we could potentially find predictors for whether a student will be in track or not by their freshman year. According to UofC:
``` “On-track students are more than three and one-half times more likely to graduate from high school in four years than off-track students” ``` 
Finding these students before their freshman year has the potential to set them on track before it is already too late.


## Sources, Spreadsheets, References 
### Normalized Data 
* [Freshman On Track](https://cps-final-project-bucket.s3.us-east-2.amazonaws.com/metrics_fot_schoollevel_2022-1.csv) 
* [Graduation and Dropouts](https://cps-final-project-bucket.s3.us-east-2.amazonaws.com/metrics_cohortgraduationdropoutadjusted_schoollevel_2011to2019+(1).csv) 
* [College Enrollment and Persistence](https://cps-final-project-bucket.s3.us-east-2.amazonaws.com/metrics_collenrollpersist_schoollevel_20222_CLEAN_3.csv) 

### Sources 
* [Chicago Public Schools Distric Data Metrics](https://www.cps.edu/about/district-data/metrics/)
* [Chicago Public Schools Assessment Reports](https://www.cps.edu/about/district-data/metrics/assessment-reports/) 
* [Data Insights (To&Through, UChicago)](https://toandthrough.uchicago.edu/sites/default/files/uploads/documents/Data%20Insights%202020%20v5_0.pdf)
