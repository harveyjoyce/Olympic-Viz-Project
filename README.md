# Olympic-Viz-Project

![Olympic_flag svg](https://github.com/harveyjoyce/Olympic-Viz-Project/assets/158076969/5276ca56-0683-4272-bbb2-a90de82f0eb3)

# 🅰️About

Welcome to the Olympic Games Data Analysis project repository! This project focuses on the comprehensive analysis of Olympic Games results using SQL and Tableau visualization. By setting up a local MySQL database, importing Olympic Games results data, and designing a custom schema to efficiently store and manage the data, this project aims to provide insightful perspectives on various aspects of the Olympic Games across different regions and over time.

[See in Tableau Public](https://public.tableau.com/app/profile/harvey.joyce/viz/OlympicsDashboardSQLProject/CountryPerformance?publish=yes)

# 🗝️Key Features

### 🛠️ Database Setup
The project involves setting up a local MySQL database environment to house the Olympic Games data efficiently.

### 📥 Data Import
Olympic Games results data is imported into the MySQL database, ensuring clean and structured storage for subsequent analysis.

### 📐 Custom Schema Design
A tailored schema is created to organize the Olympic Games data optimally, facilitating efficient querying and analysis.

### 📊 Tableau Integration
The MySQL database is seamlessly connected to Tableau for visualization and dashboard creation, enabling intuitive exploration of the Olympic Games data.

### 🔍 Exploratory Analysis
Through Tableau dashboards, the project offers in-depth exploratory analysis of individual regions' performance across various Olympic Games metrics. Moreover, key metrics are compared across different editions of the Games, providing valuable insights into trends and patterns over time.

## 📌 Data Source

I'm using an Olympic History csv. file from Kaggle, the dataset has information on each athlete that has participated in an olympic event, their result as well as other info about themselves and the games.
[Link to Kaggle](https://www.kaggle.com/datasets/bhanupratapbiswas/olympic-data?select=dataset_olympics.csv)
 
## 🛠️ Database Setup

After Downloading and installing MySQL, I created a database and a table that has all the same columns as my csv. file.


```
create database olympics;
use olympics;

CREATE TABLE `staging` (
  `ID` BIGINT,
  `Name` VARCHAR(1024),
  `Sex` VARCHAR(1024),
  `Age` VARCHAR(1024),
  `Height` VARCHAR(1024),
  `Weight` VARCHAR(1024),
  `Team` VARCHAR(1024),
  `NOC` VARCHAR(1024),
  `Games` VARCHAR(1024),
  `Year` BIGINT,
  `Season` VARCHAR(1024),
  `City` VARCHAR(1024),
  `Sport` VARCHAR(1024),
  `Event` VARCHAR(1024),
  `Medal` VARCHAR(1024),
  `NOC_Region` VARCHAR(1024),
  `NOC_notes` VARCHAR(1024)
);
```

## 📥 Data Import

Next I used a website to covert the csv. file into SQL, I then insterted that data into the staging table

```
INSERT INTO `staging` VALUES
(...

...)

```

## 📐 Custom Schema Design

Even though the table is now in MySQL, it is poorly optimised. We can create new dimension tables and relate them back to the main fact (results) table.

The dimension tables are: 
- Athletes Table (Details about the athletes, their ages, height, weight)
- Events Table (Details about the events and sport group)
- Teams Table (Details about the teams, the NOC and the region)
- Games Table (Details about the games, when and where the games were held)

When restructuring your table the goal is to make every row unique for that ID. To do this we use the SELECT DISTINCT syntax.

```
CREATE TABLE athletes  (
  athlete_id INT,
  name VARCHAR(100),
  sex VARCHAR(1),
  height INT,
  weight INT
);
INSERT INTO athletes
SELECT DISTINCT
id as athlete_id,
name,
sex,
NULLIF(height,'') as height,
NULLIF(weight,'') as weight
FROM olympics.staging;

```
We also need to add Primary and Foreign Keys

```
-- Set Primary Keys
ALTER TABLE athletes ADD PRIMARY KEY(athlete_id);
ALTER TABLE teams ADD PRIMARY KEY(team_id);
ALTER TABLE games ADD PRIMARY KEY(games_id);
ALTER TABLE events ADD PRIMARY KEY(event_id);

-- Set Foreign Keys
ALTER TABLE results ADD FOREIGN KEY (athlete_id) REFERENCES athletes(athlete_id);
ALTER TABLE results ADD FOREIGN KEY (team_id) REFERENCES teams(team_id);
ALTER TABLE results ADD FOREIGN KEY (games_id) REFERENCES games(games_id);
ALTER TABLE results ADD FOREIGN KEY (event_id) REFERENCES events(event_id);

```

I downloaded DBeaver and created an ER diagram to visualize table relationships.

![Olympic Schema](https://github.com/harveyjoyce/Olympic-Viz-Project/assets/158076969/037e09c7-56c2-4166-9d81-4cc2a8009cca)


## 📊 Tableau Integration

The MySQL database is seamlessly connected to Tableau for visualization and dashboard creation, enabling intuitive exploration of the Olympic Games data.

![Tableau Model](https://github.com/harveyjoyce/Olympic-Viz-Project/assets/158076969/620e554d-0fd5-4887-b4ef-bcda98bface6)

## 🔍 Exploratory Analysis

Through Tableau dashboards, the project offers in-depth exploratory analysis of individual regions' performance across various Olympic Games metrics. Moreover, key metrics are compared across different editions of the Games, providing valuable insights into trends and patterns over time.

### My first Dashboard looks at Country Performance:

- First, I have a series of KPIs looking at Events Competed, Medals Won and Athletes Competed, with extra KPIs breaking down medal types and gender.
- My next chart looks at how each country compares to the others in the medal table.
- There is another chart that looks at what was the country's win (gold medal) ratio for each sport group.
- The final chart looks at the country's best ranking athletes.

There is tooltip and filter functionality across the dashboard.

![Dashboard Country Performance](https://github.com/harveyjoyce/Olympic-Viz-Project/assets/158076969/89fd1cff-dcca-4783-9c26-42485a0fb0b5)

### My second Dashboard looks at Athlete Metrics:

- The chart shows each athlete's recorded age and BMI over the years. This shows the distribution and averages over time.
- This chart can be filtered for specific countries, gender, sport group or medal type.

![Dashboard Athlete Metrics](https://github.com/harveyjoyce/Olympic-Viz-Project/assets/158076969/4c46903c-2a21-4243-bc65-20a483721600)

[See in Tableau Public](https://public.tableau.com/app/profile/harvey.joyce/viz/OlympicsDashboardSQLProject/CountryPerformance?publish=yes)
