# A/B Test for E-commerce Sales

## Project Description

I have determined the effectiveness of the new banner to increase sales using an A/B test. I recommend that we must not launch the new banner if our unique objective is to increase revenue because we did not observe strong evidence that there was an increase in revenue per user. Despite that, I have found strong evidence that our conversion rate increased in the treatment. Then, I recommend to launch our banner as an effective marketing strategy and I give some extra recommendations to improve the company’s sales.

## Context 

The GloBox’s growth team ran an A/B test with a new banner (Figure 1) aiming to promote its new category of food and drinks and increase its revenue. The control group did not perceive a difference in the app and the treatment group was exposed to the banner. These sample groups have a similar size (Control: 24343 users, treatment: 24600 users) and are stratified random samples (stratas: gender, country and mobile device). The test lasted 12 days in the first quartile of 2023 which is a reasonable period to observe any change in sales for our E-commerce company.

<p align="center" > 

 <img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/A-B%20test%20groups.png" alt="A/B test groups" height="300" > 
 

Figure 1. Landing pages for the control group and the treatment group.

 </p> 

The dataset generated during the test period contains three tables shown in the ERD in Figure 2. 

<p align="center" > 

<img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/ERD.jpeg" alt="Entitity Relationship Diagram for the A/B test" height="350"> 
 
 </p>

Figure 2. Entity Relationship Diagram generated from the data collected during the A/B test.

The three tables were joined using SQL (see the code below) with an inner join between the users and groups tables and a left join between the users and activity tables in order to include all the users' IDs, even those without purchase events. 

The fields gender, purch_dt and spent contained null values. Null values for gender were due to users that prefer not to specify their gender in the sign-up form, while purch_dt and spent null entries were generated after the left join for the users’ IDs that did not make a purchase. Consequently, any null values generated can be optionally replaced with zeros for interpretation purposes without impacting the results of the A/B test.

Two KPIs were taken into account to perform the hypothesis testing:
-	Revenue per user 
-	Conversion rate 

As predefined, the significance level was set to 5% and, the distribution of the metrics and their difference (between control and treatment groups) were assumed to be normal. Additionally, as the company has not defined any practical significance for these tests, I adopted the Cohen’s d value to have an idea if the difference between the control and treatment metrics is, in fact, relevant or just originated from the size effect.  

## Results
The similarities between the bar sizes for the control and treatment groups in each category or strata confirm that our experiment contains stratified randomized data. From the plots we can also say that most of our users are people from USA and Brazil using Android devices.  

<img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/Loc_users.png" alt="Location of the users in each group"> 

Figure 3.

<img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/gender_device.png" alt="Gender an device of the users in each group"> 
Figure 4.

Revenue per user
In order to determine if there was a significant difference in revenue between the two groups, we ran a hypothesis test. We used the revenue per user as the metric and the unique users as the units. 
Using a z-test we did not find a statistically significant difference between the revenue per user for the two groups at the 5% significance level (p=0.94). The confidence interval (95%) for the difference in revenue per user between the two groups was (-0.471, 0.439). This interval includes 0. Also, note that the 95% confidence intervals for each group have a high overlap with each other (Figure 5). 

<img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/Conf_intervals_rev.png" alt="Confidence intervals for each group"> 

Figure 5. 95% confidence interval for the revenue per user in the control and treatment groups. 

There are two mean reasons why the revenue per user is not the best approach to run a hypothesis test and can lead to impractical results. First of all, most of the values for revenue are null values (or zeros) corresponding to the users that did not purchase. This displaces the mean (and also the median) to zero. Second, if we ignore these null values and see the revenue distribution, we find a lot of outliers (see Figure 6), which lead to high variances for each group. With high variances, the difference between the groups is more likely to be due to random error. A possible solution to deal with the variance of the revenue is to increase the sample size and do the A/B test again. Although we can increase the sample size until detect a significant difference, this difference could be so small to have practical significance and could be a waste of time. Thus, I do not recommend to lunch the banner on the homepage if the unique goal is to increase the revenue and I also do not recommend redoing the A/B using the same banner and including more user users or extending the test period. 

<img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/Revenue_barplot.png" alt="Distribution of the revenue: control vs treatment"> 
Figure 6. 

Conversion Rate

We obtained a better result from the hypothesis testing using the conversion rate instead of the revenue per user. Using a z-test we found a statistically significant difference between the conversion rate for the two groups at the 5% significance level (p= 1.1x 10-4). The confidence interval (95%) for the difference in revenue per user between the two groups was (0.0034, 0.010) (figure 7). This interval does not include 0. Also, note that the 95% confidence intervals for each group do not overlap each other (Figure 8).

<img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/Diff_distr_CI.png" alt=""> 
Figure 7.

<img src="https://github.com/jorgeUnas/A-B-Test-for-E-commerce-Sales/blob/main/Conf_intervals.png" alt=""> 
Figure 8. 
Finally, we have got a percentual difference of 18% in the conversion rate between the groups. As we do not have a parameter to define the practical significance of our A/B test we can use the Cohen’s d, which is a measure of the difference between the two groups in terms of standard deviation. A Cohen’s d value of 0.8 or higher is generally interpreted as a larger effect or a high practical significance. As our Cohen’s d was 5.467, we can trust that our difference in conversion rates has practical significance and consequently our banner is an effective marketing strategy. 



## Recommendations
-	Based on the results above, it does not make sense to launch the treatment because we didn’t observe an increase in revenue per user. I recommend that we do not launch it.
-	As the treatment group had a higher conversion rate compared to the control group, I highly recommend utilizing the banner as a cost-effective marketing strategy. By leveraging this approach, we can potentially achieve a double benefit: reducing marketing costs while simultaneously increasing profits. Furthermore, the higher conversion rate indicates a greater likelihood of converting website visitors into customers, ultimately translating into enhanced revenue generation. 
-	Instead of redoing the test by increasing the test period or sample sizes while using the same banner, I recommend using a different banner that contains more specific information about the new products. For instance, a banner highlighting the high food quality or, even better, emphasizing competitive prices, discounts, or promotions applied. This approach can create a sense of urgency in users, encouraging them to make a purchase.
-	We can further enhance our revenue and create engagement by implementing an A/B test that targets customers who have purchased at least one product from the new category. This test involves sending personalized emails that suggest related products or new deals based on their previous purchase. By including a call to action in these emails, we aim to not only increase our revenue but also foster a sense of engagement with our customers.








