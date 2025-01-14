# Seismic-Alert-System

-----

## Project Summary

-----

In this proyect we are are playing the role of a team designated to the study and analysis of seismilogical activity in Chile.

Making team with the United States and Japan in a project called :

Working towards global standardization of seismological networks and effective communication to the civilian community.

What we try to achieve in this project is the study of seismilogical data, and make an analysis of it's influence in the monetary and danger aspect.

We aim to achieve a convincing and helpfull analysis trough a dashboard to Chile specifically, and provide the population a web service to sort of translate what can be for the mayority of people complicated concepts regarding earthquakes.


## Objectives

-----

This project covers a wide variety of objectives, some linked to the nature of earthquakes and others to their socio-economic impact. This goals are the following:

    - Identify areas with high magnitude seismic risk with potential capacity for massive destruction, in order to concentrate investments and increase the capacity to respond to earthquakes.

    - Determine and analyze if there was evolution in the quality of constructions and preventive measures in the determined time.

    - Analyze the earthquake social impact via social media.

    - Develop a machine learning model able to predict possible secondary effects based on historical data.

    - Implement a communication system to alert the civilian population in the event of a potential earthquake

## KPIs

-----

In order to evaluate the this objectives and their potential improvements, we had to set a number of KPIs which gave us a clear undestanding of 

    - Percentage of GDP allocated in the event of an earthquake (suggested budget for seismic emergency)

    - Earthquake mortality rate by area 

    - Frequency and probability of occurrence of a major earthquake

    - Earthquake damage indicator comparizon between diferent geographical regions  




## Steps of the project

### Data Engenieering

-----

For this step in the project, we decided to build the follwing ETL process in GCP, to be later used in PowerBI and Streamlit.

![ETL_GCP](https://user-images.githubusercontent.com/107011436/204930224-579ed636-4105-48c8-8693-a98310326156.png)

First , we'll set up a Data Warehouse, wich will contain the seismic data from all three countries standarized coming from different APIs provided by the USGS, JMA and (api de chile). In this Data Warehouse will also be stored some historical data, regarding damage, population, repairing costs, tsunamis and volcano activity.

![ETL_diagram drawio](https://user-images.githubusercontent.com/107011436/204930313-15103c5f-f0e4-4939-8fc2-7ba9b3e22e55.png)

The ETL process consist in updating the data every hour, making use of a Cloud Function wich is activated by a PUB/SUB trigger, set by a Cloud Scheduler.
Once triggered, this function performns the calls to the three APIs, saves the raw data into the Cloud Storage bucket inside a folder, then transformns the data and saves it into another folder. There´s another folder in the storage containing the historical and static data, wich will be used for the analysis.

Once the data is storaged and normalized, is retrieved by BigQuery wich has an external table for the constant load of data, in order to not get duplicates intrude the following analysis, a schedule query runs every time new data is loaded into the Data Warehouse and performns the creation of a new table without the duplicates from the external table.

### Data Analysis

-----

The main focus of this project is given from an analytical point of view. From the large amount of data collected, we were able to evaluate different aspects of the earthquakes and their consequences. We are interested in being able to transmit the information obtained through dashboards in Microsoft Power BI or Streamlit.


### Machine Learning

-----

Since predicting earthquakes is quite a difficult and not so reliable thing to do, we want to be able to succesfully classify the recent earthquakes by it's damage potencial, and predict if there could be a tsunami following after.

To perform this classification we made use of a KMeans machine learning model, to clusterize the earthquakes by their magnitude and depth.

![kmeans_distr](https://user-images.githubusercontent.com/107011436/206577223-ad09e384-47bf-4425-8b39-0d28fd4f1709.png)

![kmeans_siloute_score](https://user-images.githubusercontent.com/107011436/206577252-7ab6844d-fdf6-40fd-b5b8-8d988e9b8a8a.png)

For the prediction of possible tsunamis, we used a GradientBoost machine leraning model, taking the magnitude and depth of an earthquake.

![gradient_archi](https://user-images.githubusercontent.com/107011436/206578264-2615a7d1-e6c3-49d0-8167-7bc0212e85a2.png)

- Model Accuracy:

![gradiente_scores acc](https://user-images.githubusercontent.com/107011436/206577522-f1f7fe27-5932-4716-84a7-3b581e3622b6.png)

- Model F1Score:

![gradiente_scores f1](https://user-images.githubusercontent.com/107011436/206577669-ba9e3973-263c-4e1c-99c8-7b9654993702.png)

- Model matrix with evaluation data:

![gradient_matrix](https://user-images.githubusercontent.com/107011436/206577929-6ceedbd6-f1ea-4774-85aa-7ba6545af615.png)

But how can we make this information reach the population?

We designed a web app making use of Streamlit, making the deploy online so people can get the information provided by the Data Warehouse and the models insights.

Our objetive is to give all this information in a simple and reliable way of measuring the possibilities of future damage caused by an earthquake.

### Web App

-----

![homepage](https://user-images.githubusercontent.com/107011436/206320058-dbdb8f40-c6c3-45a0-82b3-d76b5cb0401f.png)

The app provides the following features:

- Most recent earthquakes in US, Japan and Chile

Provides data of the latest earthquakes and it's percentage for country 

![last_earthquakes](https://user-images.githubusercontent.com/107011436/206320604-957568f0-893b-42d9-916b-f659dad19c4d.png)

- World Map

Providing the earthquakes locations, density distribution, and information for the user regarding it's location and filtering options

![world_map](https://user-images.githubusercontent.com/107011436/206321173-4ce7da9c-aba9-460e-8345-6047005aece9.png)

- Mayor Earthquakes Data

Data from mayor earthquakes occurred in the US, Japan and Chile. Given visuals of the data between the countries and the damages caused

![mayor_data](https://user-images.githubusercontent.com/107011436/206321654-73c3d450-3edb-4a2a-b684-53b664e72b54.png)


# Gitbook

-----

This project is currently in the development process. All changes can be followed in the following gitbook: 

https://aarons-organization.gitbook.io/earthquake-project1/
