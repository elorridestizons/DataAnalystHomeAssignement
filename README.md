# Data Analysis Test

## Overview
This technical test provides 2 years of fictitious data from Quark VPN, a fictional VPN provider. 
Your task is to analyze customer data, identify churn patterns, present your observations of the data, 
and provide recommendations for improving user retention.

We will assume these facts to simplify the exercise:
1. We are only interested in paid users here, so we are ignoring all free users.
2. Once a user cancels churns, they never come back.

The detail you prefer to go in to is up to you. However your score in this assessment 
will ultimately be comprised of an assessment of:

* **Technical skill (40%)**
  * Data cleaning and processing
  * Statistical analysis
  * Visualization quality
  * Modeling approach
  * Code quality & documentation

* **Business Understanding (30%)**
  * Insight relevance
  * Recommendation quality
  * Problem-solving approach
  * Strategic thinking

* **Communication (30%)**
  * Clear presentation of findings
  * Visualization effectiveness
  * Documentation quality
  * Technical writing

## Datasets

> All data is synthetic

### User Data (vpn_customer_data.csv)
Contains individual user information and usage patterns:

user_id,signup_date,churn_date,plan,plan_price,monthly_usage_hours,country,num_tickets,support_tickets

* user_id: Unique identifier for each user
* signup_date: Date when the user first subscribed
* churn_date: Date when the user cancelled (empty if still active)
* plan: monthly or yearly
* plan_price: price of plan
* monthly_usage_hours: Average monthly usage in hours
* country: User's signup country
* countries_connected: Number of unique countries connected to
* devices_used: Semicolon-separated list of device types used
* num_tickets: Number of support tickets raised
* support_tickets: Type of support tickets raised

### Events (vpn_events.csv)
Records significant events that might influence VPN usage:

* event_type: Type of event (employment, sporting_event, political, regulatory)
* country: Related service or platform
* date: Event date
* description: Event description
* impact_score: Estimated impact magnitude (1-5 scale)

### External Factors Data (vpn_external_factors.csv)

This file represents some external factors that could have an
influence on VPN usage including if the country is welcoming to VPNs,
the [internet freedom index](https://freedomhouse.org/countries/freedom-net/scores) of 
the country, vpn friendliness, remote work adoption, and employment rates.


Monthly country-level indicators:
* country: Country code
* month: Year and month
* internet_freedom_index: Internet freedom score (0-100)
* vpn_friendliness: Binary indicator of privacy law changes
* remote_work_adoption: Percentage of remote workers
* employment: Economic health indicator (0-100)


## Deliverables
> Jupyter notebooks or equivalent are preferred

* Analysis code (Python preferably) with documentation.
* Technical report (max 5 pages)
* Executive summary (max 1 page)
* Visualizations
