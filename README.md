# Exploratory-Data-Analysis-on-Layoffs---SQL-Project

### About the Dataset:
This Layoff dataset Consists of layoff details of around 1600 companies from March’2020 to March 2023.

### Objective
Our aim of this project is to do thorough Explanatory Data Analysis(EDA) on layoffs & finding the key insights through various dimensions.

### Source File:
The file is in excel format(File Name: layoffs.xlsx) of columns company, location, industry, total laid-off, percentage laid-off, date, stage, country & funds raised in millions.

### Exploratory Data Analysis(EDA):
EDA involves the layoff data to answer Key Questions, as follows,

Q1:  Calculate the total no. of companies, that undergoes Layoffs.

```sql
select count(distinct company) as Total_Companies
from layoffs_staging2
where total_laid_off is not null
or percentage_laid_off is not null;
```
   
Insights/Findings:

* There were 1628 companies in total, which undergone layoffs from March 2020 to March 2023 as per the dataset.

Q2. Total No. of companies in which 100% laid off happened
```sql
select count(*) as complete_Laid_off
from layoffs_staging2
where percentage_laid_off =1;
```

Insights/Findings:
* There were 116 companies in total, Where 100% laid off took place.

Q3. Find the industry & total_laid_off 
```sql
select industry, sum(total_laid_off) as total_laid_off
from layoffs_staging2
where year(`date`) is not null
group by 1
order by 2 desc;
```

Insights/Findings:
* From year 2020 till 2023, Total laid off counts are highest in the Consumer Industry, followed by Retail, Others & Transportation & so on.
















