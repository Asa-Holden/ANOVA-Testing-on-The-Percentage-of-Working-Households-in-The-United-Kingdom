# ANOVA-Testing-on-The-Percentage-of-Working-Households-in-The-United-Kingdom
## Executive Summary
This project explores an Analysis of Variance tests (ANOVA) to approve whether there’s a difference in the mean of the percentage of working households across the UK from 2006 onwards – to accompany this, another smaller 	ANOVA was produced to look into whether this same difference can be seen between England Scotland and Wales. This information is a starting point to a wider investigation around what could be causing the difference and whether its something to be worried about. Both ANOVAS produced a probability value of <0.0001, meaning that the test shows a statistically significant difference between the mean of the years and the countries. To confirm this analysis, a different type of ANOVA was conducted to test whether the variance within the years differs between the years. This both confirms an important assumption of ANOVA and provides a statistically significant base for more in-depth analysis to follow.
## Data Source and Processing
The data was sourced from the Office for National Statistics websites statistical bulletin and can be located here: Households by combined economic activity status of household members by local authority: Table A1 LA - Office for National Statistics. Although it uses official data collected by local authorities, it is not accredited as ‘official statistics’ – its instead called ‘official statistics in development’ or ‘experimental statistics’. It uses the Annual Population Survey ( APS ) to provide a data table focusing on the economic status of households across the UK, showing the working houses, mixed houses and workless houses. These figures are shown both numerically and in the form of percentages – the percentages proved easier to work with because they required less cleaning and showed approximate normal distribution across all years.
Processing the data was quite time consuming, but luckily, they were all mostly in the same format, making concatenation easy.  The data from 2006-2019 was all in one excel file on separate sheets, so it was an easy upload to Power BI – 2020, 2021 and 2022 were in separate documents in a slightly different format, so they had to be uploaded individually. To save time, I concatenated all of these sheets into one large dataset and did the rest of the data engineering on this – during the join, I made sure the origin table had its own column so I knew what data belonged to what year since I knew this would be the key to the ANOVA testing. The later years were altered by copy and pasting the values into the correct columns and then deleting any unwanted columns. The usual data cleaning tasks then had to be complete like promoting the top row to headers, removing errors and removing rows with useless information. A challenging part of the engineering was getting all the ‘number of’ columns in the same format since some were just the number and others were in thousands. <img width="975" height="86" alt="image" src="https://github.com/user-attachments/assets/05c705b8-8c2d-4463-8fbd-c38018aca896" />   
This DAX formula was created when I used the ‘alter columns from example’ function to put everything into thousands form – put simply, it deletes the last three numbers of all the numbers in a specified column. 
Once cleaned and organised by year, I imported into JMP, my chosen analytical software, and alter all of the columns to the correct data type. Within this software I used the ‘Hide and Exclude’ function to section out the big groups (United Kingdom, West Midlands, East London and so on) of each new year, since they were a sum of everything beneath it meaning the data would have very large outliers and be doubled if left in. I then used the ‘Recode’ function to create a new column to show the country based off the first letter of the ‘Area Code’ column. It also provides a very useful Recode function, that groups everything by name and allows me to find inconsistencies like some having two spaces or a difference of capital letters – although I didn’t need it this clean for my analysis, Its good practice to ensure future tests/data exploration runs smooth.
<img width="803" height="675" alt="image" src="https://github.com/user-attachments/assets/c372490d-6f6c-497b-9aef-7972f617fcc4" />
With this, the data was in the correct form for analysis, with no duplication, errors, outliers, correct data types and an abundance of numerical data to work with.
## Data Visuals and Analysis 
Before any analysis is produced, ive made a habit of running the data through a couple checks to ensure ive not missed anything in the engineering stage. These include putting the data into histograms to check for normality (Figure1), running the graph builder to check for any outliers I may have missed or any unknown/standout relationships (Figure 2) and creating a heatmap of correlations to see if everything is as expected (Figure 3). 

Figure 1:

<img width="940" height="565" alt="image" src="https://github.com/user-attachments/assets/38097643-4313-4d41-93ef-1a71e1c370a2" />

Figure 2:

<img width="940" height="540" alt="image" src="https://github.com/user-attachments/assets/0290c707-20d8-478a-8972-a014d4633d32" />

Figure 3:

<img width="1059" height="607" alt="image" src="https://github.com/user-attachments/assets/6d853aee-336a-4de3-b6c9-03c512e8eab9" />

Once I had a better understanding of the data, the intended analysis could begin. I started by creating my null hypothesis which stated: the mean percentage of working households in the UK was the same between every year. The second null hypothesis states that the mean percentage of working households was the same across the different countries. With the data cleaned and the hypothesis inplace, I could now run the analysis.

Figure 4:

<img width="1039" height="1033" alt="image" src="https://github.com/user-attachments/assets/1e48a8a1-6146-4421-9768-620e00a4bb10" />

Figure 4 shows the ANOVA test between the years. The probability produced is less than 0.05, meaning the null hypothesis would be rejected. More applicably, it means that there is a statistically significant difference between the mean percentage of working households of each year

Figure 5:

<img width="865" height="1048" alt="image" src="https://github.com/user-attachments/assets/7bb0ad91-42a8-4330-a50f-63fd4cfd21df" />

Figure 5 shows 4 tests to ensure the groups have approximately equal variances, which is an important assumption used when running an ANOVA. All tests produced a probability greater than 0.05, meaning the mean of the variances is statistically equal between all the groups. This verification step is important to allow the use of the stats in proffesional development – its also part of the reason why I chose to do an ANOVA.

Figure 6:

<img width="981" height="1024" alt="image" src="https://github.com/user-attachments/assets/23bfa787-d7b4-4b1e-a24a-1799dc656aaf" />

Figure 6 shows the ANOVA test between the countries. The probability produced is less than 0.05, meaning the null hypothesis would be rejected. More applicably, it means that there is a statistically significant difference between the mean percentage of working households of each country.
## Project Improvements
To readers without any statistical background, this analysis may appear quite daunting and complicated. To help this, I believe a starter section including a dashboard would be a good idea. The main attraction of this would be an interactive map that shows different statistics using colour. An Example of this would be a darker shade of blue if the percentage of working households is higher in a certain area. Coupling this with numbers that change depending on the selected area will allow the user to get used to the data before diving into more complicated analysis. Having a table along the bottom of the dashboard that shows working, mixed and workless household percentages when certain areas are selected would assist this change.
Focusing more on the data engineering side, I believe ‘area’ would be much more useful than ‘country’. I did try this initially, but the data wasn’t collected in a way that would allow me to produce this column without having to go through each of the 8000 rows one by one. A script to automate this would be very helpful and will be researched. This filter would allow internal comparison of midlands against outer areas and how this affects different percentages – this opens doors for deeper analysis on rural areas against cities, pointing local authorities in the correct direction of areas that may need more assistance with job seeking.
It would be interesting to see a machine learning approach to predicting how the percentage of working households may shift across different areas based on something not usually thought about like their carbon emissions. Although the data would be difficult to obtain, a study on the percentage of working households in areas with more companies chasing Net Zero compared to companies that aren’t could be eye opening to the negative effects of carbon emission policies.
Research made by Ben Caldecott, 2021, ‘A Machine Learning Model to Investigate Factors Contributing to the Energy Transition of Utility and Independent Power Produce Sectors Internationally’ [online] Available at netzeroclimate.org (Accessed 20/08/2025), dives into a similar subject on an international level, exploring the urgency of net zero and how the independent companies that don’t contribute as much to reaching net zero have a large effect overtime.

## References
