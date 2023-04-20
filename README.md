# ETL-pipeline-predict-customer-Behavioral-Interaction

## Introduction
This project builds an ETL pipeline to process 8GB of the search and content data of the FPT company, along with applying the Customer360 model to predict the behavior and interaction of customers to make reasonable decisionsThis project builds an ETL pipeline to process 8GB of the search and content data of the FPT company, along with applying the Customer360 model to predict the behavior and interaction of customers to make reasonable decisions.
<br>
![customer_360_data-2-1024x981 (3)](https://user-images.githubusercontent.com/101572443/233166379-ed883b3d-ff6d-4e8e-b9b6-722e7a663d5b.png)

## Dataset
The dataset used for processing is the content data of April and the lookup data of June and July generated when customers use FPT telecom services:
- [log content](#)
- [log search](#bỏlinkgithubvao)

## ETL process description
- Log Content <br>
The task in this section is to build an ETL to support analyze data in April and rely on customer360 to find out the user's level of interaction with the application. <br>
The 'log content' has the schema as below: <br>
![screenshot_1681929647 (1)](https://user-images.githubusercontent.com/101572443/233170588-95393779-53ec-494f-b1da-b92b7b139e95.png)<br>
And The 'log content' was transformed as followed:
  - Clean & Change table format: Dive into the '_source' data to clean them up by pivoting the 'Type' and sum the 'TotalDuration' to figure out exactly how many hours each 'Contract' spends on each category, then expand on the ideas below.
  - Most Watch: Based on the data table calculated above find out which 'Type' has the 'Most Watch' for each 'Contract' by applying the Window ranking function to find where the 'rank == 1' and pick it outputting a new column named 'Most_Watch'.
  - Taste: This column contains all 'Types' whose watch time is not NULL of each 'Contact'.
  - IQR(InterQuartile Range) type: Applying this type to exclude outliers or abnormal values in the data as well as give decide the usage frequency of the user, and assign a value to each interval. Divide total duration to 3 parts as follows:
    - TotalDuration < 25%(q1): lower
    - 25%(q1) =< TotalDuration =< 75%(q3): middle
    - TotalDuration < 75%(q3): upper
  - Activeness: This value will represent the total number of days each 'Contract' appears in the month to see how often the customer uses.
  - Clinginess: Based on 'IQR' and 'Activeness' we will know the frequency of use, thereby making predictions about the future actions of customers to come to decisions to increase usage time for customers such as giving reminders by message, calling customer care....The naming of the frequency is as follows:
    - low:
      - Activeness <= 15days & iqr_type = lower
    - medium:
      - 15days < Activeness & iqr_type = lower
      - Activeness <= 10days & iqr_type = middle
      - Activeness <= 10days & iqr_type = upper
    - high:
      - 10days <= Activeness & iqr_type = middle
      - 10days <= Activeness & iqr_type = upper <br>
- Result
    ![image](https://user-images.githubusercontent.com/101572443/233328617-091c4ab9-da1c-479c-9142-79b73ae1a9ed.png)

- Log Search <br>
The task in this section is to build an ETL to support data analysis in June and July as well as rely on customer360 to compare and find the change of customers' application usage habits in 2 months.
  - Pickup & Clean data: During this period, I pick up the user_id & keyword that is generated when the customer opens and uses the application, not when the customer closes the application
  - Most Search: It has the same process with 'Most Watch'. Take the 'keyword' has 'rank == 1' and pick it outputting a new column named 'Most_Search'.
  - Category: This stage will map between the customer's keyword and the library where the movie name and genre of a movie of FPT are stored to create a column of movie genres that customers watched in the month. (But since the library is sensitive and not shared data, I manually mapped and faked them. so from now, the data created from 'Category' will also be affected accordingly !!)
  - Behavior: This task would be divided into 2 small tasks. Find 'Behavior_Change' represent the change in viewing habits from June to July and 'Explain_Changed' is a detailed explanation for the change in column 'Behavior_Change'.
    - Behavior_Change: Shows the 'Change' or 'Unchange' which represents the customer's behavior.
    - Explain_Changed:
      - If the value in 'Behavior_Change' is 'Unchange', it keeps 'Unchange'.
      - If the value in 'Behavior_Change' is 'Change', it will display specifics and switch from what to what.
  - Trending: This column will show the top 10 most searched data for each month
  
## Results

## Requirements
- [Python 3.10.9](#)
- [Java 19.0.2](#)
- [Hadoop winunit.exe-2.7.1](#)
- [Spark 3.3.1-bin-hadoop2](https://www.youtube.com/watch?v=OmcSTQVkrvo)
- [Install Apache PySpark](https://www.youtube.com/watch?v=OmcSTQVkrvo)
