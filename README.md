# Exploratory-Data-Analysis-on-Layoffs---SQL-Project

### About the Dataset:
This Layoff dataset Consists of layoff details of around 1600 companies from March’2020 to March 2023.

### Objective
Our aim of this project is to do thorough Explanatory Data Analysis(EDA) on layoffs & finding the key insights through various dimensions.

### Source File:
The file is in excel format(File Name: layoffs.xlsx) of columns company, location, industry, total laid-off, percentage laid-off, date, stage, country & funds raised in millions.

### Exploratory Data Analysis(EDA):

Q1:  Calculate the total no. of companies, that undergoes Layoffs.

```select count(distinct company) as Total_Companies
from layoffs_staging2
where total_laid_off is not null
or percentage_laid_off is not null;```


