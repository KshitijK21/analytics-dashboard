# EV Adoption Analytics Dashboard

A cloud-based data analytics dashboard built on Amazon Web Services (AWS), visualizing global Electric Vehicle (EV) adoption behavior across 50,000 records.

---

## Live Demo

URL: http://65.0.85.233

The dashboard fetches live data directly from AWS Athena via a REST API. No manual data upload is required.

---

## Project Overview

This project was built as part of a Cloud Computing course to demonstrate the use of AWS cloud services in building a real-world data analytics pipeline. The dataset contains behavioral data of 50,000 individuals regarding EV adoption, including income, commute patterns, fuel expenses, city type, education level, and EV adoption likelihood.

The goal was to store raw data in the cloud, process and query it using serverless tools, and display insights on a live, publicly accessible web dashboard.

---

## Architecture

```
Raw CSV Dataset (50,000 rows)
          |
          v
    AWS S3 Bucket
    (dad-bucket1)
          |
          v
    AWS Glue Crawler
    (schema detection)
          |
          v
  AWS Glue Data Catalog
  (analytics_dashboard DB)
          |
          v
    AWS Athena
    (serverless SQL queries)
          |
          v
    AWS Lambda
    (Python function)
          |
          v
  AWS API Gateway
  (REST API endpoint)
          |
          v
  EC2 Web Server
  (Apache + HTML Dashboard)
          |
          v
    Live Website
  http://65.0.85.233
```

---

## AWS Services Used

| Service | Role | Details |
|---|---|---|
| Amazon S3 | Raw data storage | Stores the 50,000 row EV dataset CSV file in bucket dad-bucket1 |
| AWS Glue | Data cataloging | Crawler auto-detects schema and registers table in Glue Data Catalog |
| Amazon Athena | Serverless querying | Runs SQL queries directly on S3 data with no database setup needed |
| AWS Lambda | Backend function | Python 3.12 function that triggers Athena query and returns results |
| API Gateway | REST API | Exposes Lambda as a GET /data HTTP endpoint for the frontend |
| Amazon EC2 | Web hosting | t2.micro instance running Apache httpd serving the dashboard |

---

## Dataset

Name: Global EV Adoption Behavior 2026

Size: 50,000 records

Key Columns:

| Column | Description |
|---|---|
| age | Age of the individual |
| annual_income | Annual income in USD |
| city_type | Urban / Suburban / Rural |
| education_level | Highest education level |
| current_vehicle_type | Type of vehicle currently owned |
| daily_commute_km | Daily commute distance in km |
| weekly_travel_distance_km | Weekly travel distance in km |
| fuel_expense_per_month | Monthly fuel expense in USD |
| ev_adoption_likelihood | High / Medium / Low |
| ev_knowledge_score | Knowledge score about EVs (0-10) |
| electricity_cost_per_kwh | Local electricity cost |
| environmental_awareness_score | Awareness score (0-10) |
| government_incentive_awareness | Awareness of EV incentives |
| battery_replacement_concern | Concern about battery replacement |
| charging_station_accessibility | Access to charging stations |

---

## Dashboard Features

### Scorecards
- Total Records (live count after filters)
- Average Annual Income
- Average Daily Commute (km)
- Average Fuel Expense per Month
- Average EV Knowledge Score

### Charts

| Chart | Type | Insight |
|---|---|---|
| EV Adoption by City Type | Bar Chart | Which city types have most EV interest |
| Vehicle Type Distribution | Pie Chart | Breakdown of current vehicle types |
| Income vs Fuel Expense by Education | Grouped Bar | How education affects income and fuel spending |
| EV Adoption Likelihood | Doughnut Chart | High / Medium / Low adoption breakdown |

### Filters
- City Type (Urban / Suburban / Rural)
- Current Vehicle Type
- EV Adoption Likelihood
- Education Level

### Data Table
- Top 50 records with color-coded EV adoption likelihood badges

---

## Tech Stack

| Technology | Usage |
|---|---|
| Python 3.12 | AWS Lambda function |
| boto3 | AWS SDK for Python (Athena API calls) |
| HTML5 / CSS3 | Frontend dashboard structure and styling |
| JavaScript (ES6) | Data fetching and rendering |
| Chart.js | Interactive charts |
| Apache httpd | Web server on EC2 |

---

## How It Works

1. The raw CSV dataset is uploaded to AWS S3
2. AWS Glue Crawler scans the file and registers the schema in the Glue Data Catalog
3. AWS Athena queries the data using SQL on the Glue table
4. AWS Lambda (Python) is triggered by the API, runs the Athena query, waits for results, and returns them as JSON
5. API Gateway exposes the Lambda function as a public REST API endpoint (GET /data)
6. The HTML dashboard on EC2 calls the API on page load, receives the JSON data, and renders charts and scorecards automatically

---

## Files in This Repository

| File | Description |
|---|---|
| ev_dashboard_live.html | Main dashboard that fetches live data from AWS API |
| ev_dashboard.html | Offline version where CSV can be uploaded manually |
| README.md | Project documentation |

---

## Setup Instructions

### Prerequisites
- AWS Account with Free Tier
- IAM user with S3, Glue, Athena, Lambda, API Gateway, EC2 permissions

### Steps
1. Upload CSV to S3 bucket
2. Run Glue Crawler to catalog the data
3. Create Athena table in analytics_dashboard database
4. Deploy Lambda function with the Python code
5. Create API Gateway HTTP API with GET /data route linked to Lambda
6. Launch EC2 t2.micro instance (Amazon Linux 2023)
7. Install Apache: sudo yum install -y httpd and sudo service httpd start
8. Copy ev_dashboard_live.html to /var/www/html/index.html
9. Access dashboard at http://YOUR-EC2-PUBLIC-IP

---

## Authors

Kshitij Kadam
Varshit Maidham
Cloud Computing Course Project
AWS Region: Asia Pacific (Mumbai) - ap-south-1

---

## Notes

- EC2 instance is t2.micro (Free Tier eligible)
- Athena queries are billed per TB scanned (minimal cost for this dataset)
- Lambda is within Free Tier (1 million requests per month free)
- API Gateway is within Free Tier (1 million calls per month free)
