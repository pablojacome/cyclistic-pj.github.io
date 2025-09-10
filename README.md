Google Data Analytics Capstone
Case Study 1: How Does a Bike-Share Navigate Speedy Success?
📌 Project Overview

This project is part of the Google Data Analytics Capstone. The goal is to analyze Cyclistic bike-share data to understand the differences in usage between casual riders and annual members, and to propose marketing strategies that increase annual memberships.

🎯 Business Task

Identify key differences in how casual riders and annual members use Cyclistic bikes, in order to design a marketing campaign aimed at converting casual riders into annual members.

📂 Data Sources

Public Cyclistic trip data from August 2021 to July 2022.

Twelve monthly CSV files merged into a single dataset.

Data was downloaded and stored locally.

The dataset is publicly available and used for educational purposes only.

🧹 Data Cleaning & Manipulation

All data preparation and transformation were performed in SQL Server due to the large volume of records.

Key steps included:

Applied TRIM() function to remove extra spaces in station names.

Removed all NULL values.

Calculated ride length and ride distances.

Filtered out inconsistent data (e.g., trips lasting more than 2 days but covering less than 1 km).

Used the physics formula (velocity = distance/time) and an average biking speed of 10 km/h as a logic check to remove unrealistic data.

👉 The SQL scripts used for cleaning and transformation are attached in this repository.

📊 Data Analysis

The analysis focused on uncovering behavior differences between user types by:

Determining the total number of rides by rider type.

Identifying monthly trends.

Identifying weekly trends.

Analyzing differences by bike type and station location.

Summarizing results into aggregated tables using AVG(), MIN(), MAX(), and GROUP BY.

📈 Visualization

Data visualization was performed using Tableau Public.
🔗 Interactive Dashboard

✅ Recommendations

Based on the analysis, the following marketing strategies are recommended:

Launch annual membership discount campaigns during summer, the season with the highest demand for both casual and member riders.

Focus marketing efforts on top stations used by casual riders:

Streeter Dr & Grand Ave

DuSable Lake Shore Dr & North Blvd

DuSable Lake Shore Dr & Monroe St

Target Sundays and Mondays, when casual riders show the highest usage, with tailored campaigns to encourage membership conversion.

🛠️ Tools & Technologies

SQL Server → Data cleaning, manipulation, and analysis.

Tableau Public → Data visualization and dashboards.

Excel/CSV → Data storage and preliminary checks.

👤 Author

Pablo Jácome

BI Analyst | Data Analyst | Chemical Engineer, MBA
