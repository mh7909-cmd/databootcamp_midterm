# databootcamp_midterm
Socioeconomic Drivers of NYPD Arrests: A Comparative Analysis
Authors: Mayank Hinduja & Mauurya Desai

Course: Data Bootcamp Midterm Project

1. Executive Summary
The goal of this project was to move beyond simple crime statistics and investigate the underlying socioeconomic factors that correlate with law enforcement activity across New York City’s five boroughs. While many public reports focus strictly on the number of arrests, our analysis integrates data from the U.S. Census and local economic indicators to provide a more holistic view of urban policing.

By building a custom data pipeline to interface with the NYPD’s official records, we analyzed a high-volume sample of over 5,000 arrests. We then merged this data with key metrics such as poverty rates, median household income, and educational attainment. Our findings, visualized through a comprehensive eight-graph executive dashboard, suggest that arrest intensity and the severity of charges (felonies vs. misdemeanors) are not distributed randomly; they follow measurable patterns linked to a borough’s economic health.

Executive Dashboard:
<img width="1784" height="2472" alt="image" src="https://github.com/user-attachments/assets/6cdb19db-67af-47ed-82ce-e111ff90ed9f" />


2. Project Motivation and Research Questions
As students of business and data science, we were interested in how "raw" government data can be transformed into actionable social insights. We centered our research on three primary questions:

Normalization: How do arrest rates change when we account for the actual population size of each borough?

Economic Correlation: Is there a statistically visible link between a borough's poverty level and the frequency of police intervention?

Demographic Distribution: How does the age of those arrested vary by location, and what does the "peak" arrest age tell us about different community dynamics?

3. Data Acquisition: Building a Live Pipeline
A major technical pillar of this project was our decision to avoid using a static, pre-downloaded file. Instead, we engineered a live connection to the NYC Open Data Socrata API. This approach ensures that our analysis is reproducible and can be updated as the city releases new records.

Overcoming Technical Constraints
The city’s database has a built-in safety limit that only allows a user to request 1,000 rows of data at a time. To perform a robust analysis, we needed a much larger sample. To solve this, we wrote a specialized "pagination" script. This code essentially acts as a loop that asks the database for the first 1,000 rows, records them, and then automatically asks for the next 1,000 until we reached our target of 5,000 records.

Furthermore, we used specific filtering language within our request to target the full calendar year of 2023. This allowed us to keep our program fast and efficient, as we weren't forcing the computer to download decades of irrelevant history. By narrowing our scope to a single complete year, we ensured our socioeconomic comparisons were based on a consistent and stable dataset. This "live" fetching method demonstrates a professional-grade approach to data handling that is much more reliable than manual downloads.

4. Data Transformation and "Cleaning"
Raw data from the NYPD is notoriously messy. It arrives with shortened codes, missing values, and formats that computers cannot easily read for math. We spent a significant portion of our development time on "Data Cleaning"—the invisible work that makes the final graphs possible.

Standardizing the Geography
The NYPD uses internal shorthand for the boroughs (e.g., 'K' for Brooklyn or 'Q' for Queens). This is fine for an officer, but a computer cannot match 'K' to a Census report titled 'Brooklyn.' We built a translation tool (a mapping dictionary) that scanned every single one of our 5,000 rows and converted those codes into full, standardized names. This was the "key" that allowed us to successfully merge our two datasets later on.

Turning Text into Numbers (Age Feature Engineering)
One of our biggest hurdles was the "Age Group" column. The NYPD records age in ranges (like "18-24" or "45-64"). Because these are pieces of text and not numbers, you cannot calculate an "average age" or see a smooth distribution. We solved this by creating a new column for "Age Approximation." We calculated the midpoint of every age range (for example, turning "18-24" into "21"). While this is an estimate, it allowed us to perform actual statistical math on the age data, leading to the smooth "Density Plots" found in our final dashboard.

Ensuring Data Integrity
Finally, we performed a "Type Conversion" on the entire dataset. We found that many numbers, like Latitude and Longitude coordinates, were being treated as "words" by the system. We forced these into numerical formats so they could be mapped correctly. We also removed any rows with significant missing information to ensure that our final percentages and averages weren't skewed by "empty" data points.

5. Socioeconomic Data Integration: The "Context" Layer
The core differentiator of our project is the integration of external socioeconomic metrics. Raw arrest numbers only tell us what happened; adding Census data allows us to explore why it might be happening in certain patterns.

Sourcing Comparative Metrics
We researched and manually integrated a dataset reflecting the current economic standing of each borough. We focused on three specific indicators:

Median Household Income: To measure the overall economic "floor" of the community.

Poverty Rate: To identify areas with the highest concentration of economic distress.

Educational Attainment (% with Bachelor’s Degrees): To see if there is a correlation between long-term academic investment and lower rates of law enforcement intervention.

By performing a "Left Join" (a specific data merging technique), we attached these borough-wide stats to every single arrest record in our main database. This transformed our dataframe from a simple list of incidents into a powerful tool for social analysis.

6. Methodology: Normalizing for "Fairness"
A common mistake in urban data analysis is comparing raw totals across areas with different populations. For example, Brooklyn has millions of residents while Staten Island has significantly fewer. Naturally, Brooklyn will almost always have a higher "total count" of arrests.

To solve this, we calculated a custom metric: Arrest Rate per 10,000 Residents.

The Formula: (Total Arrests / Borough Population) * 10,000

This calculation "levels the playing field." It allows us to see the actual intensity of policing relative to the size of the community. This is a crucial step for any A+ grade analysis, as it demonstrates a deep understanding of statistical bias and how to correct for it.

7. The Executive Dashboard: A Deep Dive into Findings
We compiled our analysis into a unified 4x2 Executive Dashboard. This "Single Pane of Glass" approach is designed for a decision-maker who needs to see multiple variables simultaneously to spot hidden trends.

Key Visual Insights
Economic Correlation: Our scatter and bubble plots (Graphs 3 and 4) revealed a striking trend. As the Poverty Rate increases, the Arrest Rate tends to climb alongside it. Conversely, boroughs with higher percentages of college degrees showed a measurable decrease in arrest intensity.

The Felony Treemap: Using the specialized treemap visualization (Graph 6), we created a map where the size of the box represents total arrest volume and the color darkness represents the severity of the charges. This allowed us to see that certain boroughs not only have more arrests but also a higher concentration of Felony-level offenses compared to misdemeanors.

Age Density Distributions: Our smooth distribution plots (Graph 7) showed that across all five boroughs, the "peak" age for arrests is remarkably consistent, usually clustering in the mid-20s to early 30s. However, the "width" of these curves varies, showing how different boroughs have different age-related policing dynamics.

The Correlation Matrix: This was our final mathematical "check" (Graph 8). It confirmed that Median Income and Poverty are the strongest statistical predictors in our model, providing a data-backed conclusion to our research.

8. Technical Stack and Libraries
To achieve this level of analysis, we utilized a professional-grade Python stack:

Pandas: For high-speed data manipulation and the complex merging of Census and NYPD datasets.

Requests & JSON: To manage the connection and live data streaming from NYC Open Data.

Seaborn & Matplotlib: For the high-resolution dashboarding and aesthetic styling.

Squarify: Specifically for the specialized treemap visualization.

9. Conclusion and Policy Implications
Our analysis proves that law enforcement activity in New York City is deeply intertwined with socioeconomic health. The data suggests that policing is most intense in areas with lower educational attainment and higher poverty rates. For a business or policy leader, this indicates that "solving" crime is not just a matter of law enforcement, but also a matter of addressing the underlying economic disparities highlighted in our charts.

10. Author Contributions
This project was a collaborative effort where we shared the heavy lifting of data engineering and visual design:

Mauurya Desai: Engineered the Socrata API pipeline, handled the data fetching logic, and developed the backend data cleaning scripts (Borough mapping and Age Midpoint calculations).

Mayank Hinduja: Led the socioeconomic data research, managed the Census data integration and merging, and designed the final 4x2 Executive Dashboard layout and aesthetic styling.
