# databootcamp_midterm
Socioeconomic Drivers of NYPD Arrests: A Comparative Analysis
Authors: Mayank Hinduja & Maurya [Last Name]

Course: Data Bootcamp Midterm Project

1. Executive Summary
The goal of this project was to move beyond simple crime statistics and investigate the underlying socioeconomic factors that correlate with law enforcement activity across New York City’s five boroughs. While many public reports focus strictly on the number of arrests, our analysis integrates data from the U.S. Census and local economic indicators to provide a more holistic view of urban policing.

By building a custom data pipeline to interface with the NYPD’s official records, we analyzed a high-volume sample of over 5,000 arrests. We then merged this data with key metrics such as poverty rates, median household income, and educational attainment. Our findings, visualized through a comprehensive eight-graph executive dashboard, suggest that arrest intensity and the severity of charges (felonies vs. misdemeanors) are not distributed randomly; they follow measurable patterns linked to a borough’s economic health.

2. Project Motivation and Research Questions
As students of business and data science, we were interested in how "raw" government data can be transformed into actionable social insights. We centered our research on three primary questions:

Normalization: How do arrest rates change when we account for the actual population size of each borough?

Economic Correlation: Is there a statistically visible link between a borough's poverty level and the frequency of police intervention?

Demographic Distribution: How does the age of those arrested vary by location, and what does the "peak" arrest age tell us about different community dynamics?

3. Data Acquisition: Building a Live Pipeline
A major technical pillar of this project was our decision to avoid using a static, pre-downloaded file. Instead, we engineered a live connection to the NYC Open Data Socrata API. This approach ensures that our analysis is reproducible and can be updated as the city releases new records.

Overcoming Technical Constraints
The city’s database has a built-in safety limit that only allows a user to request 1,000 rows of data at a time. To perform a robust analysis, we needed a much larger sample. To solve this, we wrote a specialized "pagination" script. This code essentially acts as a loop that asks the database for the first 1,000 rows, records them, and then automatically asks for the next 1,000 until we reached our target of 5,000 records.

Furthermore, we used specific filtering language within our request to target only the most recent data from 2023 and 2024. This allowed us to keep our program fast and efficient, as we weren't forcing the computer to download decades of irrelevant history. This "live" fetching method demonstrates a professional-grade approach to data handling that is much more reliable than manual downloads.

4. Data Transformation and "Cleaning"
Raw data from the NYPD is notoriously messy. It arrives with shortened codes, missing values, and formats that computers cannot easily read for math. We spent a significant portion of our development time on "Data Cleaning"—the invisible work that makes the final graphs possible.

Standardizing the Geography
The NYPD uses internal shorthand for the boroughs (e.g., 'K' for Brooklyn or 'Q' for Queens). This is fine for an officer, but a computer cannot match 'K' to a Census report titled 'Brooklyn.' We built a translation tool (a mapping dictionary) that scanned every single one of our 5,000 rows and converted those codes into full, standardized names. This was the "key" that allowed us to successfully merge our two datasets later on.

Turning Text into Numbers (Age Feature Engineering)
One of our biggest hurdles was the "Age Group" column. The NYPD records age in ranges (like "18-24" or "45-64"). Because these are pieces of text and not numbers, you cannot calculate an "average age" or see a smooth distribution. We solved this by creating a new column for "Age Approximation." We calculated the midpoint of every age range (for example, turning "18-24" into "21"). While this is an estimate, it allowed us to perform actual statistical math on the age data, leading to the smooth "Density Plots" found in our final dashboard.

Ensuring Data Integrity
Finally, we performed a "Type Conversion" on the entire dataset. We found that many numbers, like Latitude and Longitude coordinates, were being treated as "words" by the system. We forced these into numerical formats so they could be mapped correctly. We also removed any rows with significant missing information to ensure that our final percentages and averages weren't skewed by "empty" data points.
