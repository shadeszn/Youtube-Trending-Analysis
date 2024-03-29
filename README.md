# Youtube-Trending-Analysis
## Introduction
In this project we will do a deep dive into the YouTube Trending video data and draw comparisons and valuable insights from it. To determine the year’s top-trending videos, YouTube uses a combination of factors including measuring users interactions (number of views, shares, comments and likes). Trending videos need not be the most-viewed videos overall for the calendar year. While studying this data we will be able to look into various information, such as the top categories of the trending videos, the regional preferences in videos, the average amount of likes, dislikes and views on a trending video etc.

## Use Cases
Analyzing YouTube trending video data can provide valuable insights into popular content, viewer preferences, engagement patterns, and emerging trends. Here are some use cases where analyzing YouTube trending video data can be beneficial:

* Analyzing YouTube trending video data can help content creators and publishers understand the types of videos that resonate with viewers. By examining the attributes, such as video length, title structure, tags, and thumbnails, of trending videos, creators can optimize their own content to increase visibility, engagement, and viewership.
* The data can provide insights into the demographics, interests, and behaviors based on the region of the audience consuming popular content. This information can be leveraged to understand target demographics and create content that aligns with audience preferences.
* Content creators and marketers can identify emerging topics, themes, or formats that are gaining popularity. This information can be used to create timely and relevant content to capture audience attention and stay ahead of competitors.
* Analyzing YouTube trending video data can assist advertisers and brands in identifying trending videos that align with their target audience and brand values. It can also help understand the competitors' marketing strategies by examining their successful video content. This helps many companies to inform cost effective and widespread ad camppaigns on YouTube.

## Dataset
The data used for this project has been taken from the [Trending YouTube Video Statistics](https://www.kaggle.com/datasets/datasnaek/youtube-new) dataset on Kaggle. This dataset several months of the daily record for the top trending videos on YouTube from various regions. For convenience of data processing and handling, We have chosen Canada, USA and Great Britain (basically regions where the video titles are predominantly in English, as to maintain uniformity accross the data) as the regions from this dataset.

## Architecture

![project architecture](https://github.com/shadeszn/Youtube-Trending-Analysis/blob/main/project-architecture.jpg)

## Workflow
__1. Data ingestion:__ The raw dataset from Kaggle repository was downloaded to a local source. A new bucket was created in AWS S3 and the data was uploaded to the bucket from the local source via AWS Command Line Interface (CLI).

__2. Data processing:__ The reference data in our dataset was in a semi-structured JSON format. This type of data is not suitable for querying operations. In order to process this raw data, we have created a Python function in AWS Lambda that will convert this JSON file to an Apache Parquet file. By converting JSON to Parquet within Lambda, we can transform the data into a more optimized format before storing it in a data lake or data warehouse. Services such as AWS Glue and Athena natively support Parquet as an input format. Processing this data helps to seamlessly integrate our data with these services and take advantage of the advanced querying capabilities. The cleaned data was then stored in a separate S3 bucket.

__3. Metadata cataloging:__ We have used AWS Glue crawler to scan and discover the CSV files from the raw data S3 bucket source. Glue is able to understand and extract meaningful information from the data, such as the file format, structure, data types and relation between tables. This is collectively known as metadata. The metadata makes it easier to analyze the available data. We can set triggers for the Crawler to run whenever new data is added to the bucket, so that we can maintain an up-to-date catalog of our data.

__4. ETL processing:__ In the next step we have created a job in AWS Glue that will take the raw CSV data from our source. These CSV files containing YouTube data from each region were processed for the purpose of cleaning (i.e., handling NULL values, mapping columns to a uniform data type etc.). After this the data was converted into Apache Parquet format and the output was stored in our clean data bucket. Glue jobs can leverage the metadata catalog that we previously created to define proper tables and schemas.

__5. Preparing data for analytics:__ At this stage we have our cleaned YouTube trending regional data and the statistical reference data both in Parquet format. These two tables need to be joined for using in ananlytics. The INNER JOIN operation between these was performed through a Glue Job and the output table was stored in a separate bucket for analytics, ready to use.

__6. Creating dashboard / visualization:__ Using AWS QuickSight we have transformed the data into meaningful and valuable insights in the form of different graphs, charts, figures etc. The dashboard can be found [here](https://github.com/shadeszn/Youtube-Trending-Analysis/blob/main/analytics-dashboard.pdf).

## Analysis Samples

![image](https://github.com/shadeszn/Youtube-Trending-Analysis/assets/60690784/6171174c-b171-41d9-98f2-e4ca67168a1a)

![image](https://github.com/shadeszn/Youtube-Trending-Analysis/assets/60690784/c0fb7966-d6c6-4dde-a7ce-a01b0311ae6c)

![image](https://github.com/shadeszn/Youtube-Trending-Analysis/assets/60690784/00f9ab7c-e06a-4d35-b9d6-8e99046891b3)

## Tech Stack

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

Services include S3, AWS Glue, Athena, Lambda and QuickSight.
