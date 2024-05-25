# Exploratory-Data-Analysis-on-Layoffs---SQL-Project

### About the Dataset:
This Layoff dataset Consists of layoff details of around 1600 companies from March’2020 to March 2023.

### Objective
Our aim of this project is to do thorough Explanatory Data Analysis(EDA) on layoffs & finding the key insights through various dimensions.

### Source File:
The file is in excel format(File Name: layoffs.xlsx) of columns company, location, industry, total laid-off, percentage laid-off, date, stage, country & funds raised in millions.

### Exploratory Data Analysis(EDA):
EDA involves exploring the layoff data to answer Key Questions, as follows,

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
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/a6bacfa4-ed51-44d5-a87e-9f31da3b8f7b)
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/a45b0df5-10ca-4a7d-abe4-fda2802cac7d)
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/c769513a-1d12-46d3-b3e6-e72f0fde7b66)

Insights/Findings:
* From year 2020 till 2023, Total laid off counts are highest in the Consumer Industry, followed by Retail, Others & Transportation & so on.
* Consumer Industry has the highest total Laid off with 45,182 count out of which Google ranked No1 with Layoff count of 12,000, followed by Meta,Twitter& so on.
* Amazon ranked No1 in Retail Industry with highest layoff count as 18.2k.
* Microsoft & Ericsson has the layoff counts of 10k & 8k in Other Industry Category.

Q4. Find the Total_laid_off by Each Year 
```sql
select year(`date`) as year, sum(total_laid_off) as total_laid_off
from layoffs_staging2
where year(`date`) is not null
group by 1
order by 1 desc;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/96260de2-5da1-45d0-bcbe-fb251e53dce4)

Insights/Findings:
* In 2022, the layoff count peaked at the highest level of 160,000 among all the years.
* However, the year 2023 has seen an almost 125,000 layoff count based on the data available for the first three months, suggesting that it could potentially surpass the peak if the trend continues until December 2023.

Q5. Find the Total_laid_off count by each stage.
```sql
select stage, sum(total_laid_off) as total_laid_off
from layoffs_staging2
group by 1
order by sum(total_laid_off) desc;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/356d8fee-407d-411a-aa66-fc8cff6f2305)

Insights/Findings:
* 204k people were laid off during the post-IPO stage. This occurred due to cost-cutting actions taken by companies as they transitioned from private to public status in the stock market.

Q6.  Calculating top 5 company based on total Layoff for each year
```sql
with company_year as (
select company, year(`date`) as years, sum(total_laid_off) as total_laid_off
from layoffs_staging2
group by 1,2
order by 2 desc
), company_rank as
(select *, dense_rank() over (partition by years order by total_laid_off desc) as ranking
from company_year
where years is not null)
select *
from company_rank
where ranking <= 5;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/d2323de6-6574-4182-ae85-e941178c3c8b)

Insights/Findings:
* In 2020, data shows that most layoffs were in the Transportation & travel industry due to the widespread impact of the coronavirus pandemic affecting countries worldwide.
* Subsequently, from 2021 onwards, layoffs occurred not only in the consumer and retail industries but also in other sectors.

Q7. Calculating top 5 country based on layoff total for each year.
```sql
with country_year as (
select country, year(`date`) as years, sum(total_laid_off) as total_laid_off
from layoffs_staging2
group by 1,2
order by 2 desc
), country_rank as
(select *, dense_rank() over (partition by years order by total_laid_off desc) as ranking
from country_year
where years is not null)
select *
from country_rank
where ranking <= 5;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/10f9fc8d-9229-4871-a346-7b4b0568445b)

Insights/Findings:
* For subsequent years from 2020 to 2023, United States ranked No1 country in Layoffs.

Q8. Calculating top 5 industry based on layoff total for each year
```sql
with industry_year as (
select industry, year(`date`) as years, sum(total_laid_off) as total_laid_off
from layoffs_staging2
group by 1,2
order by 2 desc
), industry_rank as
(select *, dense_rank() over (partition by years order by total_laid_off desc) as ranking
from industry_year
where years is not null)
select *
from industry_rank
where ranking <= 5;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/49858060-ca61-4674-acd1-09d804088e8e)

Insights/Findings:
* In 2020, data shows that most layoffs were in the Transportation & travel industry due to the widespread impact of the coronavirus pandemic affecting countries worldwide.
* Subsequently, from 2021 onwards, layoffs occurred not only in the consumer and retail industries but also in other sectors.

Q9. Find the no. of companies that doesnot raise any Funds/ unknown and 100 % layoff.
```sql
select company, industry,country,total_laid_off, 
		percentage_laid_off, funds_raised_millions, stage
from layoffs_staging2
where funds_raised_millions is null and percentage_laid_off = 1
order by funds_raised_millions desc;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/f0109bcf-2c3d-4577-9719-ea358560e87c)

Insights/Findings:
* There are 21 Companies in total, where 100% laid-off happened & the funds raised is Unknown & most of them are from US & India.

Q10.  Find the top 5 mass layoff companies within a day
```sql
select company,industry, country, `date`,
max(total_laid_off) as max_layoff
from layoffs_staging2
where total_laid_off is not null
group by 1,2,3,4
order by max(total_laid_off) desc limit 5;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/0e126aef-c260-44ec-99bf-bab7a3864eb8)

Insights/Findings:
* Mass layoffs took place in the months of November 2022 and January 2023 at Google, Meta, Microsoft, and Amazon in the United States, with each company laying off more than 10,000 people within a day.

Q 11. Calculate the total layoff, each month of the year in ascending order.
```sql
select substr(`date`,1,7) as Month,
sum(total_laid_off) as total_layoff
from layoffs_staging2
where total_laid_off is not null
and substr(`date`,1,7) is not null
group by 1
order by 1 asc ;
```
![image](https://github.com/Aarthi-14/Exploratory-Data-Analysis-on-Layoffs---SQL-Project/assets/147639053/3dbb0148-7e3b-4de8-9a8a-945705b6cef8)

Insights/Findings:
* Mass layoffs took place in the months of November 2022 and January 2023 with Layoff counts of 53K and 85K. 

### Key Insights:
* Mass layoffs took place in the months of November 2022 and January 2023 with Layoff counts of 53K and 85K. 
* From year 2020 till 2023, Total laid off counts are highest in the Consumer Industry, followed by Retail, Others & Transportation & so on.
* Consumer Industry has the highest total Laid off with 45,182 count out of which Google ranked No1 with Layoff count of 12,000, followed by Meta,Twitter& so on.
* Amazon ranked No1 in Retail Industry with highest layoff count as 18.2k.
* In 2022, the layoff count peaked at the highest level of 160,000 among all the years. However, the year 2023 has seen an almost 125,000 layoff count based on the data available for the first three months, suggesting that it could potentially surpass the peak if the trend continues until December 2023.
* 204k people were laid off during the post-IPO stage. This occurred due to cost-cutting actions taken by companies as they transitioned from private to public status in the stock market.
* In 2020, data shows that most layoffs were in the Transportation & travel industry due to the widespread impact of the coronavirus pandemic affecting countries worldwide. Subsequently, from 2021 onwards, layoffs occurred not only in the consumer and retail industries but also in other sectors.
* For subsequent years from 2020 to 2023, United States ranked No1 country in Layoffs.
* Mass layoffs took place in the months of November 2022 and January 2023 with Layoff counts of 53K and 85K. 


Analysis by Aarthi Duraisingam, Data Analyst
[Linkedin]((https://www.linkedin.com/in/aarthi-duraisingam-2438b7248/))
Date: 15/05/24



















