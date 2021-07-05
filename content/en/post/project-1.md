---
date: 2021-04-09T10:58:08-04:00
description: "Exploratory Data Analysis (EDA) and Data visualization"
featured_image: "/images/Car-accident.jpg"
tags: ["Data Analytics"]
title: "What Can You Tell from 3 Million Traffic Accident Records in US"
---
On average, there are 6 million car accidents in the U.S. every year. That's
roughly 16,438 per day. Of these crashes, 22,471 caused only property damage.
Over 37,000 Americans die in automobile crashes per year. Where do the majority
of accidents happen? How the weather affect the accident severity? Which cities
have the most accident occurrences?

This project is to use EDA (Exploratory Data Analysis) and data visualization
to analyze around 3 million traffic accident records. The dataset is available
from the popular data science competition website,
[Kaggle](https://www.kaggle.com/sobhanmoosavi/us-accidents). The dataset was
collected from an approximately 4-year period using multiple APIs. These APIs
broadcast traffic events captured by various entities, hence the dataset is
considered a representation of country wide traffic information. A quick look
at the sample entries:

{{< figure src="/images/project-1-sample-data.JPG">}}

There are quite a few features in the dataset, such as accident severity rating,
location, weather condition, and surrounding traffic signs and structures. The
severity rating is defined as the impact on traffic with 1 the smallest and 4
the most significant. With the description in the table metadata from the data provider, we can
understand that it contains information including: 1. accident information; 2.
location information; 3. weather conditions; 4. traffic-related information; 5.
auxiliary information. Here are some sample entries:

Combined with the table schema, we can tell that numerical columns such as
'Distance', 'Temperature(F)' have physical meaning. Now let's take a look at
the statistics especially on those numerical columns:

{{< figure src="/images/project-1-stats.JPG">}}

We notice that there might be quite a few missing values in "Precipitation", and
"Distance" shows that first 50% of data are all 0 mi, which needs to be paid
attention later. Let's check the missing values:

{{< figure src="/images/project-1-missing-values.png">}}

It reinforces our guess: there are a large amount of data missing in "Precipitation"
as well as "Number" and "Wind_Chill". The exact proportion is shown below:

{{< figure src="/images/project-1-missing-per.png">}}

No doubt are we going to drop those features with more than 25% missing values. The rest of
entries with null values will be discarded as well.

The final count of effective entries are 2,331,076. Let's take a look at where
the accidents happen:

{{< figure src="/images/project-1-map-all.png">}}

Now we are curious about the first 10 state with most accident occurrences.

{{< figure src="/images/project-1-10-states.png">}}

Similarly, we can show the first ten cities with most accident records:

{{< figure src="/images/project-1-10-cities.png">}}

We want to take a quick look at how many cities in first ten states with the most
accident records.

{{< figure src="/images/project-1-10-state-cities.png">}}

We can see that the number of accidents in cities is quite different. For
example, Texas is the one with the third highest number of accident records.
However, the number of cities in Texas is only the 6th. That means, the number
of accidents per city on average in Texas is relatively larger than most other
states. So now let's take a look at the number of accidents distribution among
cities.

{{< figure src="/images/project-1-accidents-dist.png">}}

Now we can clearly see that most of cities have only a few accident records.
We are curious how much those account for? A convenient way is to divide
the cities into intervals of different number of accident records:

{{< figure src="/images/project-1-accidents-dist-pie.png">}}

Based on the pie chart, we can split the dataset into three strata: 1. the cities with small number
of accident records, which is lower than 1,000, and 2. the cities with
moderate number of accident records, which is larger than 1,000 but smaller than
10,000, and 3. the rest of cities, where the accidents number is extremely high.
Each stratum accounts for around 1/3 of the
total entries. Now we can analyze each stratum one by one, by starting from
exploring the severity of accidents in the first stratum. Here is the number of
accidents at each severity rating:

{{< figure src="/images/project-1-stratum1-severity-count.png">}}

That means the majority of accidents' severity is rated at 2, which indicates a
slight impact on traffic such as delay. We wonder if it is somehow related to
the temperature, so let's take a look:

{{< figure src="/images/project-1-stratum1-severity-temp.png">}}

This means the temperature may not be a strong factor that determines the
severity. How about the time in a day? Does daytime or nighttime matter?

{{< figure src="/images/project-1-stratum1-severity-temp-daynight.png">}}

It looks like a correct representation of real life: temperatures tend to be
lower in nighttime than daytime. But we won't be able to know how much the time in a day
affect the accident severity.

The next question is, what about the precipitation? Unfortunately, the column
for precipitation is dropped due to too many missing values. However, we may
want to analyze the data with precipitation information later. Similarly, what
about the wind speed?

{{< figure src="/images/project-1-stratum1-severity-wind-daynight.png">}}

There are a few outliers with wind speed way larger than 100 mph, which are
ignored in the charts. We do not see a strong correlation between the accident
severity and wind speed. Interestingly, the wind speed tends to be higher in
daytime. Now we can take similar method to explore other factors such as
visibility and traffic distance:

{{< figure src="/images/project-1-stratum1-severity-visib-dist-daynight.png">}}

It seems that most of visibility distance is 10 miles, which strongly indicates
that the driving condition is very good (it makes sense otherwise we may have had
a lot of accidents!). For the traffic impact distance, it only serves as a
reference because the impact is caused by the accident not the other way.
Therefore, for later predictive analysis, the distance should not be used.

Now we can take a look at the number of accidents in different weather conditions.

{{< figure src="/images/project-1-stratum1-weather-counts.png">}}

The categories of weather conditions are a bit rather qualitative, ambiguous and
even arbitrary. For example, there is no definition to distinguish what is
considered as "Mostly Cloudy" and "Overcast". Therefore, we will use the five
most frequent weather conditions to compare with the accident severity.

{{< figure src="/images/project-1-stratum1-severity-weather-per.png">}}

It seems that weather conditions such as 'Fair' or 'Clear' may dictate the
severity rating, but without quantitative definition, we can't make further conclusion.

Let's now take a quick look at the correlations among numerical values. Selected
features include: Distance, Temperature, Humidity, Pressure, Visibility and Wind speed.

{{< figure src="/images/project-1-stratum1-pair.png">}}

We can see that the variables of weather conditions are not tightly correlated
with each other. This can be observed from the Pearson coefficient calculation as well.

{{< figure src="/images/project-1-stratum1-corr.png">}}

To make it more straightforward visually, we can only display the Pearson
coefficient falling in the range of larger than 0.5 or smaller than -0.4.

{{< figure src="/images/project-1-stratum1-corr-strict.png">}}

It is safe to say that all numerical variables do not show any significant
correlations with one another.

Now let's explore the other two strata. To make it simple and also to avoid repetition, we
will only check 1. the number of accidents at each severity rating; 2. the
relationship between the temperature and severity rating; 3. weather conditions
effect; 4. correlation of numerical variables. Here is the analysis of the second
stratum, i.e. the cities with moderate number of accident records larger than
1,000 but smaller than 10,000.

{{< figure src="/images/project-1-stratum2.png">}}

Below is the analysis for the third stratum:

{{< figure src="/images/project-1-stratum3.png">}}

From the above analysis, we can have a few conclusions based on the 3 million
traffic accident records in US:
* Accident records from the dataset are distributed extremely uneven among cities.
Therefore, we decided to split the data into three strata based on the number
of accident in cities.
* Significantly more accidents are rated at severity 2 than the ones at the rest
of ratings.
* Temperatures collected close the accident scene do not have strong indication
on the severity rating. The difference between daytime and nighttime is unclear
since it is aligned with the typical representation of temperature change.
* The weather conditions might be a potential indicator of the severity rating.
However, due to the lack of quantitative definition, the use of such indicator is
questionable.
* All of the numerical quantities do not show any strong correlation with one
another. They are assumed to be treated as independent variables.
