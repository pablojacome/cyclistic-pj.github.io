# Google Data Analytics Capstone
## Case Study 1: How does a bike - share Navigate Speedy Success? 

### Scenario: 

The director of marketing from Cyclistic believes the company’s future success depends on maximizing the number of annual memberships. Therefore, the team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, the team will design a new marketing strategy to convert casual riders into annual members.

### Statement of business task:

To identify the differences in the use of Cyclistic between casual and member riders in order to launch a marketing campaign aimed to casual riders that allows them to convert into member riders.

### Data sources used:

1) I downloaded the records available by Cyclistic from August 2021 to July 2022 and  stored them on the hard drive disk (HDD) of my computer.
2) I used SQL Server to join the twelve files (one per each month of year) into one unique dataset. Before this step, it was necessary to change data type from certain columns.
3) The data used in this analysis are only for academic purposes. Apparently, data belongs to Clyclistic, but there is no way to confirm that since it is public. 
4) The data used in this analysis is enough to stand out main differences between casual and member riders behavior. 
5) When I sorted and filtered the table I realized that there are many rows showing NULL values. 

### Data cleaning and manipulation:

1) The cleaning, manipulation and analysis process was performed in SQL Server. This was because of the large amount of data.
2) I used the TRIM() function in order to remove any space character in the Station Name Column.  
4) During cleaning and manipulation process I made the following assumptions:
5.1) Remove all NULL values. Under my perspective this rows wouldn't contribute to the analysis.
6.2) After calculating the ride length and the distances covered by each ride, I realized there were inconsistent data. Like taking more than two days to travel less than 1 kilometer. Under this situation I searched for the average speed of a normal rider (10 km/h). So, using the velocity equation
(velocity = distance/time) I removed all those information that didn't match the physics logic. 

Note: you can check the sql code attached to this file. 

### Data analysis:

1) Determine the total number of rides during the period of time selected. Aggregate functions like AVG(), MAX() and MIN() were used. It was GROUPED BY type of rider.
2) Determine the different behavior between month of the year and type of rider.
3) Determine the different behavior between day of the week and type of rider.
4) Determine the different behavior between location and type of rider. 
5) Summarize the whole analysis into a unique table grouped by tipe of rider, rideable type, day of the week, month of year and station name. 

### Visualization:

1) The Visualization process was performed in Tableau Public. Click on the following link to visualize it:
https://public.tableau.com/views/CapstoneGoogleDataAnalytics/Dashboard12?:language=es-ES&:display_count=n&:origin=viz_share_link

### Recommendations:

After all the analysis process executed, I recommend to the marketing agency:
1) Launch annual membership discount campaigns during the summer. Since this is the season where there is the greatest demand for either casual and member riders.
2) Any type of campaign pretended must be conducted from the stations: Streeter Dr & Grand Ave, DuSable Lake Shore Dr & North Blvd and DuSable Lake Shore Dr & Monroe St. Since those stations are the most popular by casual riders.
3) Casual riders show greater interest in the use of bicycles on Sundays and Mondays. It would be a good idea to have a rapprochement with them during these days.

