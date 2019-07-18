# A/B Testing for an Pre-enrollment Intervention in Udacity 

## Experiment Description

At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". 

If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first.

If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.  

### Experiment Goal 

To improve the overall student experience and completion rate of Udacity courses through allocating coaches' capacity to support students who are likely to complete the course.

### What is A & B ?

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. 
The difference between experiment and control group will occur after clicking the start free trial.

At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot ![Experiment Screenshot](https://github.com/moggirain/A-B-Testing-for-Udacity-Courses/blob/master/Final%20Project-%20Experiment%20Screenshot.png) shows what the experiment looks like.

What are A & B versions in this experiment? This screenshot analyzes the A & B versions , the experiment and control group cases ![A&B](https://github.com/moggirain/A-B-Testing-for-Udacity-Courses/blob/master/AB%20experiment.png)

### Hypothesis

We hypothesized that free trial screener set clearer expectations for students upfront and thus reduce the number of frustrated students who left the free trial because they didn't have enough time— without significantly reducing the number of students to continue pass the free trial and eventually complete the course. 

### Unit of Diversion 

The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward.

The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

## Metrics

Here two types of metrics are selected for a successful experiment: Invariant and Evaluation metrics.

#### Invariant metrics

***Invariant metrics*** are used for sanity checks or A/A experiment before running the experiment, such as checking if the distributions are the same between control and experiment group, to make sure our experiment is not inherently wrong. Invariant metrics usually have a larger unit of diversion, randomly selected, or happens before the experiment starts.  

| Metric Name  | Metric Formula  | Dmin  | Notation |
|:-:|:-:|:-:|:-:|
| Number of Cookies in Course Overview Page  | # unique daily cookies on page | 3000 cookies  | C_k |
| Number of Clicks on Free Trial Button  | # unique daily cookies who clicked  | 240 clicks | C_l |
| Free Trial button Click-Through-Probability  |C_l / C_k | 0.01  | CTP | 

**Number of Cookies:** The number of unique cookies to visit the course overview page.  This is the unit of diversion and even distribution amongst the control and experiment groups is expected.  

**Number of Clicks:** The number of users (tracked as unique cookies at this stage) to click the free trial buttion. 

**Click-through-probability:** Unique cookies to click the "start free trial" button per unique cookies to view the course overview page. 

In this case, the goal of measurement is how many students will allocate more than 5 hours a week for Udacity courses, which happens before students enrolling in the courses. And thus the clicks and cookies related metircs are for the invariant metircs.User-id, however, will be tracked after enrolling the course,which is not effective. <br>

Here, we are concerned about whether students click the "free trial" and if students' clicks for the answer to questions about the number of hours they will devote to the course, and thus clicks and cookies are important. Number of cookies is used to tell whether the change is from the questions or not. 


#### Evaluation Metrics

***Evaluation metrics*** are the metrics in which we expect to see a change, and are relevant to the business goals we aim to achieve. For each metric we state a Dmin - which marks the minimum change which is practically significant to the business. For instance, stating that any increase in retention that is under 2%, even if statistically significant, is not practical to the business.


| Metric Name  | Metric Formula  | Dmin  |
|:--------:|:--------:|:--------:|
| Gross Conversion   |  #enrolled / C_l  | 0.01  | 
| Retention   | #paid / #enrolled  | 0.01  |
| Net Conversion  |  #paid / C_l  | 0.0075 |

**Gross Conversion:**  This is the number of user-ids to complete checkout and enroll in the free trial per unique cookie to click the "start free trial" button.  dmin = 0.01

**Retention:**  This is the number of user-ids to remain enrolled past the 14 day trial period, making at least one payment, per number of user-ids to complete checkout.  The practical minimum difference (dmin) = 0.01

**Net Conversion:** this is the number of user-ids to remain enrolled past the 14 day trial, making at least one payment, per the number of unique cookies to click the "start free trial" button.  dmin = 0.0075

Here, CTP won't be a good evaluation metrics, given it can not distinguish the change of free trial between the experiment and control group. Gross Conversion will be a good metric. Gross conversion means the number of enrolled divided by number of clicks. And thus in the experiment group, we hypothesized the number of enrollment will decrease after answering the screener questions, given those who selected <5 hour will not be encouraged to enroll. 

Retention is number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of enrolled students. Udacity could use this metric to check if the the number of paid students are decreasing or not by this test.If the hypothesis is true, the number of students who try free trial reduces without reducing the number of students who are enrolled past 14 days. This means that retention will increase. 
Net conversion is number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the ”Start free trial” button. This metric is also necessary because of the same reason described in Retention. If the hypothesis hold true, the number of students who remain enrolled past 14 days won’t change so much. Therefore, this metric also doesn’t change.

#### Unused Metrics

**Number of user-ids:** The number of users to enroll in the free trial.  This is not a suitable invariant metric and while it could be used as an evalution metric. User-ids are tracked only after enrolling in the free trial and equal distribution between the control and experimental branches would not be expected.  


## Measuring Standard Deviation

**_Analytical Estimate of Standard Deviation_**

| Evaluation Metric | Standard Deviation |
|:-------------------:|:--------------------:|
| Gross Conversion  | .0202 |
| Retention         | .0549 |
| Net Conversion    | .0156 |

The analytical estimate of standard devation tends to be near the empirically determined standard deviation for those cases in which the unit of diversion is equal to the unit of analysis.  This is the case for Gross Conversion and Net Conversion, but not Retention.  If we do ultimately decide to use Retention, then we should calculate the empirical variability.

## Sizing

The following calcations are based on [baseline conversion data](data/baseline_vals.csv).

### Number of Samples vs. Power

I did not deploy the Bonferroni correction for the estimate. 

**_Pageviews for Each Evaluation Metric to Achieve Target Statistical Power_**

#### Gross Conversion

- Baseline Conversion: 20.625%
- Minimum Detectable Effect: 1%
- alpha: 5%
- beta: 20%
- 1 - beta: 80%
- sample size = 25,835 enrollments/group
- Number of groups = 2 (experiment and control)
- total sample size =  51,670 enrollments
- clicks/pageview: 3200/40000 = .08 clicks/pageview
- pageviews = 645,875

#### Retention

- Baseline Conversion: 53%
- Minimum Detectable Effect: 1%
- alpha: 5%
- beta: 20%
- 1 - beta: 80%
- sample size = 39,155 enrollments/group
- Number of groups = 2 (experiment and control)
- total sample size = 78,230 enrollments
- enrollments/pageview: 660/40000 = .0165 enrollments/pageview
- pageviews = 78,230/.0165 = 4,741,212

#### Net Conversion 

- Baseline Conversion: 10.9313%
- Minimum Detectable Effect: .75%
- alpha: 5%
- beta: 20%
- 1 - beta: 80%
- sample size = 27,413 enrollments/group
- Number of groups = 2 (experiment and control)
- total sample size = 54,826
- clicks/pageview: 3200/40000 = .08 clicks/pageview
- pageviews = 685,325

_Pageviews Required:  4,741,212_


### Duration vs. Exposure

If we divert 100% of traffic, given 40,000 page views per day, the experiment would take about 119 days.

This duration with full diversion is too time consuming for running an experiment and presents some potential risks for business, such as frustrated students, lower conversion and retention, and inefficient use of coaching resources, and the opportunity riks of performing other experiments.

Therefore, if we eliminate retention, we are left with Gross Conversion and Net Conversion, and the pageview requirement will be reduced to to 685,325, and an about 18 day experiment with 100% diversion and or 35 days given 50% diversion.In terms of timing, an 18 day experiment is more reasonable. 


## Experiment Analysis

The experimental data can be found in the following links:

- [experiment group](data/experiment_data_20190713.csv)
- [control group](data/control_data_20190713.csv)

### Sanity Checks

For invariant metrics we expect equal diversion into the experiment and control group.  We will test this at the 95% confidence interval.

| Metric | Expected Value | Observed Value | CI Lower Bound | CI Upper Bound | Result |
|:------:|:--------------:|:--------------:|:--------------:|:--------------:|:------:|
| Number of Cookies | 0.5000 | 0.5006 | 0.4988 | 0.5012 | Pass |
| Number of clicks on "start free trial" | 0.5000 | 0.5005 | 0.4959 | 0.5042 | Pass |
| Click-through-probability | 0.0821 | 0.0822 | 0.0812 | 0.0830 | Pass |


### Result Analysis

95% Confidence interval for the difference between the experiment and control group for evaluation metrics.

| Metric | dmin | Observed Difference | CI Lower Bound | CI Upper Bound | Result |
|:------:|:--------------:|:--------------:|:--------------:|:--------------:|:------:|
| Gross Conversion | 0.01 | -0.0205 | -.0291 | -.0120 | Satistically and Practically Significant |
| Net Conversion | 0.0075 | -0.0048 | -0.0116 | 0.0019 | Neither Statistically nor Practically Significant |


### Sign Tests

| Metric | p-value for sign test | Statistically Significant @ alpha .05? |
|:------:|:--------------:|:--------------:|
| Gross Conversion | 0.0026 | Yes |
| Net Conversion | 0.6776 | No |

## Summary

The purpose of conducting this experiment was to test if the free trial screener could improve learning experience and completion rates. Udacity students were diverted by cookies into experiment and control group. The students in the experiment group will be directed to ask their devoted time to the course after clicking a "start free trial button", and suggested to enroll or use the free course materials while the control group will see the regular enrollment button or use the free material in the homepage.We selected three invariant metrics (Number of Cookies, Number of clicks on "start free trial", and Click-Through-Probability)as our metrics for validation and sanity check. The evaluation metrics we selected are Gross Conversion (enrollment/cookies) and Net Conversion (payments/cookies). There are two requirements to performence. The requirement run the experiment are:

1) The null hypothesis is rejected for both evaluation metircs, meaning there is no difference in the evaluation metrics between the groups. 

2) A practical signifcance threshold was set for each metric should be met, meaning the minimun difference should exceed the practical signficance threshold.

We decided not to use the Bonferonni correction, given our acceptance criteria requires statiscally signifcant differences for ALL evaluation metrics, and so the use of the Bonferonni correction is not appropriate. The Bonferonni correction is a method for controlling for type I errors (false positives) when using multiple metrics in which relevance of ANY of the metrics matches the hypothesis. In this case the risk of type I errors increases as the number of metrics increases (signifcance by random chance). In our case in which ALL metrics must be relevant to launch, the risk of type II errors (false negatives) increases as the number of metrics increases, so it stands to reason that controlling for false positives is not consistent with our acceptance criteria.The results revealed the equal distribution of cookies into the control and experimental groups for the invariant metrics, at the 95% CI. A difference in gross conversion was found to be statistically signficant at the 95% CI and was practically significant, and thus the null hypothesis was rejected. However, the Net Conversion was found to be neither statistically nor practically signficant at the 95% CI.

## Recommendation

We expect filtering students as a function of study time commitment would improve the overall student experience and the coaches' capacity to support students who are likely to complete the course, without significantly reducing the number of students who continue past the free trial.A statistically and practically signficant decrease in Gross Conversion was observed but with no significant differences in Net Conversion. This translates to a decrease in enrollment not coupled to an increase in students staying for the requisite 14 days to trigger payment.

Given these two reason, I suggest not launching the experiment but to pursue other experiments.

## Follow-Up Experiment

With the goal of improve learner experience and increase the completion and retention rate, there are a variety of strategies, such as using the badge system to motivate students sustain longer, using the return-fee-along-with-the-time-to-stay-in-the-class. For example, students are prepaid the course fee for certain time, and if they sustain longer, the system will return or deduct the fee as the reward for retention.

I suggest to design an experiment that provides the over-course submission option for students that minimize the time cost for some students. Students who choose to go for enrollment has the chance to submit the course assignments and tasks within the whole course period, even if they have late submission. For those students who do not have the average time of 5h a week but may have a packed time to complete the course, such as the weekend, will be a better choice. This gives those students, especially the working professionals a chance to complete the course.

The experiment would function in the following manner.

**Setup**: Upon enrollment students will either be randomly assigned to a control group in which they will be given the chance to submit the homework at a rigid deadline, or an experiment group in which they are notified as long as they submit the assignments within the course period, they will receive the completion certificate.

**Null Hypothesis**: Allowing the cross-course submission will not increase the number of students enrolled beyond the 14 day free trial period by a significant amount.

**Unit of Diversion**: The unit of diversion will be user-id as the change takes place after a student creates an account and enrolls in a course.

**Invariant Metrics**: The invariant metric will be user-id, since an equal distribution between experiment and control would be expected as a property of the setup.

**Evaluation Metrics**: The evaluation metric willl be Retention. A statistically and practically significant increase in Retention would indicate that the change is succesful.

If a statistically and practically signifcant positive change in Retention is observed, assuming an acceptable impact on overall Udacity resources (setting up and maintaining teams will require resource use), the experiment will be launched.

Credits to the awesome reference:
https://github.com/baumanab/udacity_ABTesting#summary
