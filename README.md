# Comprehensive Analysis of COVID-19 Data Using SQL

## Introduction:
The **COVID-19** pandemic brought about unprecedented challenges globally, prompting data professionals to play a pivotal role in understanding and managing the crisis. In this project, I undertook a thorough analysis of COVID-19 data using **SQL**, aiming to shed light on the spread of the virus, its impact on populations, and the effectiveness of vaccination efforts.

## Methodology:
I approached the analysis by leveraging SQL to manipulate and derive insights from two datasets: CovidDeaths and CovidVaccinations. My goal was to create a comprehensive picture of the pandemic's progression. I structured the project into several stages, each addressing specific aspects of the pandemic.
The Data was obtained from [Our World Data](https://ourworldindata.org/covid-deaths), and the data ranges from 1st January 2020 to 23 August 2023.

### 1. Analysis of Cases and Deaths:
In the initial phase, I retrieved and examined the data for different locations and dates. I calculated essential metrics such as total cases, new cases, total deaths, population, death percentage, and infection percentage. This allowed me to identify countries with the highest infection rates relative to their population and countries with the highest death counts.

### 2. Global Daily Numbers:
To understand the daily trends, I aggregated new cases and deaths globally by date. Calculating the death percentage offered insights into the severity of the virus over time. I also accounted for potential division by zero scenarios when calculating percentages.

### 3. Analysis on Vaccination:
Shifting the focus to vaccinations, I combined data from both tables to analyze the relationship between population and vaccination rates. By comparing the total population with new vaccinations, I gained insights into the progress of vaccination efforts worldwide.

### 4. Using CTEs to Store Values:
To enhance code readability and simplify calculations, I used Common Table Expressions (CTEs) to store intermediate results. This streamlined the querying process and allowed me to compute the percentage of vaccinated individuals in each country specifically in Nigeria.

### 5. Temporary Table and Views:
I utilized a temporary table to store aggregated data and created views for easy data retrieval. These views encapsulated key metrics like the percentage of vaccinated population, infected percentage, countries with the highest death counts, and continent-wise death counts. Creating views facilitated faster access to insights.

## Discoveries:
Throughout the analysis, several compelling discoveries emerged. Notably, certain countries exhibited remarkably high infection rates when compared to their population size. These insights emphasized the need for targeted intervention strategies. Additionally, the vaccination data showcased encouraging progress in immunization efforts, especially in densely populated regions.

#### Below is a breakdown of the discoveries:
* The Total Number of Cases worldwide within the period selected for analysis was **769,789,653**  and for Nigeria was **266,675**.
* Total Deaths recorded worldwide was **6,961,717** and **3,155**.
* Around **4.17%** of the worldwide population were confirmed infected.
* Of all the infected populations, around **0.9%** died.
* Countries like Cyprus, and San Marino had the highest population infected, with **73%** and **72%** of their population infected respectively.
* United States has the highest death count with around **1,127,152** deaths.
* United States, Brazil, India, Russia, and Mexico recorded the highest number of deaths.
* Europe had the highest total cases per continent, followed by Asia, North America, and South America.
* Europe had the total number of deaths recorded as per continent, followed by North America, Asia, and South America.

## Conclusion:
This project exemplifies the power of data analysis in tackling real-world challenges. By combining SQL skills with an understanding of public health data, I gained valuable insights into the COVID-19 pandemic's dynamics. The methodologies employed, from initial data extraction to final insights, demonstrated the iterative and investigative nature of data science. This experience further fuels my aspiration to become a data scientist and contribute to data-driven solutions that make a positive impact on society.
