# databootcamp_midterm
Socioeconomic Drivers of NYPD Arrests: A Comparative Analysis

Authors: Mayank Hinduja & Mauurya Desai

1. Executive Summary

The goal of this project was to move beyond simple crime statistics and investigate the underlying socioeconomic factors that correlate with law enforcement activity across New York City’s five boroughs. While many public reports focus strictly on the number of arrests, our analysis integrates data from the U.S. Census and local economic indicators to provide a more holistic view of urban policing.

By building a custom data pipeline to interface with the NYPD’s official records, we analyzed a high-volume sample of over 5,000 arrests from the full calendar year of 2023. We then merged this data with key metrics such as poverty rates, median household income, and educational attainment. Our final findings, visualized through a comprehensive eight-graph executive dashboard, suggest that arrest intensity and the severity of charges (felonies vs. misdemeanors) are not distributed randomly; they follow measurable patterns linked to a borough’s economic health.

Executive Dashboard:
<img width="1784" height="2472" alt="image" src="https://github.com/user-attachments/assets/6cdb19db-67af-47ed-82ce-e111ff90ed9f" />

2. Project Motivation and Research Questions
   
As students of business and data science, we were interested in how "raw" government data can be transformed into actionable social insights. We believe that data without context is just noise; therefore, we centered our research on three primary questions designed to provide that context:

Normalization: How do arrest rates change when we account for the actual population size of each borough?

Economic Correlation: Is there a statistically visible link between a borough's poverty level and the frequency of police intervention?

Demographic Distribution: How does the age of those arrested vary by location, and what does the "peak" arrest age tell us about different community dynamics?

3. Data Acquisition: Building a Live Pipeline
   
A major technical pillar of this project was our decision to avoid using a static, pre-downloaded CSV file. Instead, we engineered a live connection to the NYC Open Data Socrata API (SAPI). This approach ensures that our analysis is reproducible and can be updated as the city releases new records.

Overcoming Technical Constraints
The city’s database has a built-in safety limit that only allows a user to request 1,000 rows of data at a time. To perform a robust analysis, we needed a much larger sample. To solve this, we wrote a specialized "pagination" script. This code acts as a loop that asks the database for the first 1,000 rows, records them, and then automatically asks for the next 1,000 (using the $offset parameter) until we reached our target of 5,000 records.

Furthermore, we used specific filtering language within our request to target the full calendar year of 2023. This allowed us to keep our program fast and efficient, as we weren't forcing the computer to download decades of irrelevant history. This "live" fetching method demonstrates a professional-grade approach to data handling.

4. Data Transformation and "Cleaning"
   
Raw data from the NYPD arrives with shortened codes, missing values, and formats that computers cannot easily read for math. We spent a significant portion of our development time on "Data Cleaning"—the invisible work that makes the final graphs possible.

Standardizing the Geography
The NYPD uses internal shorthand for the boroughs (e.g., 'K' for Brooklyn or 'Q' for Queens). We built a translation tool (a mapping dictionary) that converted those codes into full, standardized names. This was the "key" that allowed us to successfully merge our two datasets later on.

Turning Text into Numbers (Age Feature Engineering)
The NYPD records age in text-based buckets (e.g., "18-24"). Because these are pieces of text, you cannot calculate an "average" or a smooth distribution. We solved this by creating an "Age Approximation" column. We calculated the midpoint of every age range (turning "18-24" into "21"). While this is an estimate, it allowed us to perform actual statistical math, leading to the smooth "Density Plots" found in our final dashboard.

5. SECTION 1: Demographic Profiling
   
Research Objective: To establish a baseline demographic profile of individuals arrested in NYC.

In this phase, we focused on the individual demographics: Age, Sex, and Race.

The "Peak" Arrest Age: Our analysis found that across all five boroughs, the highest density of arrests consistently clusters among individuals in their late 20s and early 30s.

Offense Severity: We analyzed how different demographic groups are represented across Felonies and Misdemeanors, establishing whether certain groups were being charged with more serious crimes regardless of their location.

6. SECTION 2: Socioeconomic Objectives
   
Research Objective: To conduct a multivariate analysis determining the extent to which neighborhood-level factors predict policing volume.

This represents the "Deep Dive" of our project. We researched and manually integrated a dataset reflecting the current economic standing of each borough, focusing on three indicators:

Median Household Income: To measure the economic "floor."

Poverty Rate: To identify areas with high economic distress.

Educational Attainment: To see the correlation between academic investment and police intervention.

7. Methodology: Normalizing for "Fairness"
   
A common mistake in urban data analysis is comparing raw totals. Brooklyn has millions of residents while Staten Island has significantly fewer. Naturally, Brooklyn will almost always have a higher "total count" of arrests. To solve this, we calculated a custom metric: Arrest Rate per 10,000 Residents.

The Formula: (Total Arrests / Borough Population) * 10,000

This calculation "levels the playing field." It allows us to see the actual intensity of policing relative to the size of the community.

8. The Executive Dashboard: A Deep Dive into Findings
   
We compiled our final analysis into a unified 4x2 Executive Dashboard.

Economic Correlation (Graphs 3 & 4): As the Poverty Rate increases, the Arrest Rate tends to climb. Conversely, boroughs with more college degrees showed a decrease in arrest intensity.

The Felony Treemap (Graph 6): This specialized visualization showed that certain boroughs not only have more arrests but also a higher concentration of Felony-level offenses.

The Correlation Matrix (Graph 8): This confirmed that Median Income and Poverty are the strongest mathematical predictors in our model.

9. Conclusion and Policy Implications
    
Our analysis proves that law enforcement activity in New York City is deeply intertwined with socioeconomic health. The data suggests that policing is most intense in areas with lower educational attainment and higher poverty rates. For a policy leader, this indicates that "solving" crime is not just a matter of law enforcement, but also a matter of addressing underlying economic disparities.

10. Author Contributions
    
Mayank Hinduja: Lead on Socioeconomic Analysis (Section 2). Managed Census data integration, data merging, and designed the 8-graph Executive Dashboard.

Mauurya Desai: Lead on Data Engineering (Section 1). Engineered the Socrata API pipeline, pagination logic, and handled demographic data cleaning/Age Midpoint engineering.
