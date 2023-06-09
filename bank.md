<img src="images/SQL Banking Project(1).jpg?raw=true"/>

# World Bank - SQL Financial Banking Project


For this case study, I was tasked to do some initial exploratory analysis with SQL as a financial analyst with the World Bank. 

A few questions with this analysis include: 

How many total transactions?  
How many total transactions per country? 
What is the max owed to the IDA?  
Who has the most loans?
Which was the most recent to pay?               


## **Key Insights**
- Total transactions: $1,140,120.00
- Total transactions per country (top 5):

| | country	      |  t_transactions |
| ------------- |:-------------:| -----:|
|1 | Bangladesh       | $42,026  |
|2 | Burkina Faso     |  $22,080 |
|3 | Benin		        |  $17,858 |
|4 | Bolivia	        |  $17,802 |
|5 | Afghanistan	    |  $14,708 |

- Max owed to IDA: $20,713,474,676,596.793
- The borrower with most loans:  Ministry of Eco, Planning and RegionDev
- The most recent pay date was on 12/28/2022 

## **Data Details**
The financial data comes from the [World Bank Group Finances website](https://finances.worldbank.org/Loans-and-Credits/IDA-Statement-Of-Credits-and-Grants-Historical-Dat/tdwh-3krx). The dataset contains historical snapshots of the IDA Statement of Credits and Grants (including the snapshot available as of 03/11/2023 when downloaded). The World Bank complies with all sanctions applicable to World Bank transactions updated monthly.

(from the website)

The International Development Association (IDA) credits are public and publicly guaranteed debt extended by the World Bank Group. IDA provides development credits, grants, and guarantees to its recipient member countries to help meet their development needs. Credits from IDA are at concessional rates. Data are in U.S. dollars - calculated using historical rates. 

The dataset includes: 
1.15M rows and 30 columns where each row is a - Credit or Grant

Data types

datetime - End of Period,  First Repayment Date,  Last Repayment Date, Agreement Signing Date, Board Approval Date,  Effective Date (Most Recent), Closed Date (Most Recent), Last Disbursement Date 

float - Service Charge Rate, Original Principal Amount, Cancelled Amount, Undisbursed Amount, Borrower's Obligation, Sold 3rd Party,  Repaid 3rd Party, Due 3rd Party, Credits Held, Disbursed Amount, Repaid to IDA

text - Credit Number, Region, Country Code,  Country,  Borrower, Credit Status, Currency of Commitment, Project ID, Project Name, Due to IDA,           Exchange Adjustment

## **Set-Up**
I used [Bit.io](http://bit.io/), an online SQL editor.  I uploaded the World Bank data, ran the queries, and output the results. 
It took a bit of time to load because of the number of rows in the file, so smaller datasets would not take long.

## **Analysis**
To get familiar with the dataset, I ran the query to view all the information by running SELECT * to view all the rows and columns. The SQL Editor limits the number of rows displayed, so for most queries, only about 20 rows are displayed - by adding the LIMIT 20 to the query statement.
```sql
--Return all of the table
SELECT *
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
```
To start, how many total transactions were there during this period? 

```sql
SELECT COUNT(*)
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv";
```
<img src="images/total_transactions.png?raw=true"/>


Next, I wanted to review how many total transactions were there per country.
```sql
SELECT country, COUNT(*) AS t_transactions
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
GROUP BY "country"
LIMIT 20;
```
output (display only top 5)

| | country	     |  t_transactions |
| ------------- |:-------------:| -----:|
|1 | Afghanistan |	14708 |
|2 | Africa			 |	2891 |
|3 | Albania		 |	9656 |
|4 | Angola			 |	3377 |
|5 | Armenia		 |	9137 |

How much is owed (in total) to the IDA?
```sql
SELECT SUM("Due to IDA") 
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv";
```
<img src="images/total_owed_ida.png?raw=true"/>

Then, I wanted to see how much was owed to the IDA across the different regions.
```sql
SELECT region, "Due to IDA" AS due 
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
LIMIT 20;
```
output

| | region	      |  due |
| ------------- |:-------------:| -----:|
|01 | EUROPE AND CENTRAL ASIA      | $4,097,459.75 |
|02 | EUROPE AND CENTRAL ASIA | $1,124,430.75 |
|03 | EUROPE AND CENTRAL ASIA | $5,037,689.79 |
|04 | WESTERN AND CENTRAL AFRICA | $7,828,915.06 |
|05 | WESTERN AND CENTRAL AFRICA| $19,694,655.83|
|06 |WESTERN AND CENTRAL AFRICA | 24,843,836.86 |
|07 | WESTERN AND CENTRAL AFRICA | $22,837,967.27|
|08 | LATIN AMERICA AND CARIBBEAN | $48,023,147.04|
|09 | WESTERN AND CENTRAL AFRICA | $113,806,679.36 |
|10 | WESTERN AND CENTRAL AFRICA | $15668747.75 |
|11 | EUROPE AND CENTRAL ASIA | $3,271,646.12|
|12 | WESTERN AND CENTRAL AFRICA | $40,661,934.72 |
|13 | EASTERN AND SOUTHERN AFRICA |9,698,105.12|
|14 | EASTERN AND SOUTHERN AFRICA | 12,031,455.91 |
|15 | EASTERN AND SOUTHERN AFRICA | $67,421,121.03 |
|16 | SOUTH ASIA | $68,726,988.29 |
|17 | WESTERN AND CENTRAL AFRICA | $48,841,059.52 |
|18 | WESTERN AND CENTRAL AFRICA | $53,735,643.65 |
|19 | WESTERN AND CENTRAL AFRICA | $18,523,483.76 |
|20 | EUROPE AND CENTRAL ASIA | $13252322.45 |


To get into the details a bit more, I wanted to focus on how much is owed to the IDA - I decided to look into: 
- Adding the borrower, country to know where the borrower is from, and where borrower owes more than $0
- and sorted by the amount owed by using ORDER BY "Due to IDA" in descending order
- but was interested in just reviewing the top 5 borrowers that owed the most.

```sql
SELECT borrower, country, "Due to IDA"
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
WHERE  "Due to IDA" > 0
ORDER BY "Due to IDA" DESC
LIMIT 5;
```
output

| borrower		                    |	country	| Due to IDA |
| ------------- |:-------------:| -----:|
| CONTROLLER OF AID ACCOUNTS & AUDIT |	India	 | $793,256,127.64  |
| CONTROLLER OF AID ACCOUNTS & AUDIT |	India	 |	$791,481,384.5  |
| CONTROLLER OF AID ACCOUNTS & AUDIT |	India	 | 	$787,142,516.42 |
| CONTROLLER OF AID ACCOUNTS & AUDIT |	India	 |	$782,879,864.56 |
| The National Treasury and Planning |	Kenya	 |	$780,555,197.55 |

What is the max owed to the IDA?
```sql
SELECT MAX("Due to IDA") AS max_IDA_Owed
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv";
```
<img src="images/max_owed_ida.png?raw=true"/>

Looking into a bit more details - how much is owed by country?
```sql
SELECT country, MAX("Due to IDA") AS Max_Due
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
GROUP BY country
ORDER BY Max_Due desc
LIMIT 20
```

| | country			| max_due |
| ------------- |:-------------:| -----:|
|01 | India			| $793,256,127.64 |
|02 | Kenya			| $780,555,197.55 |
|03 | Nigeria	  | $715,372,514.35 |
|04 | Pakistan	|	$6,406,033,920.00 |
|05 | Ethiopia  | $621,352,567.00 |
|06 | Ukraine   | $586,644,394.79 |
|07 | Bangladesh| $522,165,403.40 |
|08 |Congo, Democratic Republic of| $502,306,751.05 |
|09 | Uzbekistan | $500,000,000.00 |
|10 | Myanmar | $423,303,090.80 |
|11 | Vietnam | $418,952,018.23 |
|12 | Cote d'Ivoire | $359,029,810.00 |
|13 | Tanzania | $323,606,690.18 |
|14 | Uganda | $316,959,213.49 |
|15 | Ghana | $314,142,048.00 |
|16 | Nepal |$296,430,855.21 |
|17 | Senegal | $290,323,230 |
|18 | Zambia | $275,000,000.00 |
|19 | China | $260,974,560.00 |
|20 | Mali | $231,138,250.00 |


What is the average service charge rate for a loan? 
```sql
SELECT AVG("Service Charge Rate") AS Avg_srv_charge
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
```
<img src="images/avg_service_charge.png?raw=true"/>

I got a quick ad-hoc request to check the service charges for Cote d'Ivoire that are greater than $1
```sql
SELECT *
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
WHERE country = 'Cote d''Ivoire' AND "Service Charge Rate" > 1
ORDER BY "Service Charge Rate" DESC 
LIMIT 20;
```
output
<img src="images/service_charge_cotedivoire.png?raw=true"/>

Now back to the list of questions.
Who has the most loans? 
To answer this question - I had to play around with the columns a bit.
So, to get the borrower who has the most loans, I wanted to narrow down borrowers who still owed money and had the most loans.

```sql
SELECT Borrower, country, "Project Name", 
       COUNT("Due to IDA") AS count_ida, SUM("Due to IDA") AS ida_due, SUM("Repaid to IDA") AS ida_paid
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
GROUP BY Borrower, country, "Project Name"
HAVING SUM("Due to IDA") > 0
ORDER BY count_ida DESC, ida_due DESC
LIMIT 20
```
output
<img src="images/most_loans.png?raw=true"/>

Next, the last question to be answered is - Which was the most recent to pay?
For this question - there are not too many details, so first, I needed to review all the columns that are datetime data types. 
<img src="images/datetime.png?raw=true"/>

From the data dictionary, the most likely datetime columns that need to be reviewed include:
<img src="images/data_dictionary.png?raw=true"/>

After several iterations of tweaking the query – the query and results are:
```sql
SELECT Borrower, country, "End of Period", "Last Disbursement Date"
FROM "IDA_Statement_Of_Credits_and_Grants__Historical_Data.csv"
WHERE "First Repayment Date" IS NOT null 
     AND "Last Disbursement Date" IS NOT null
GROUP BY 1, 2, 3, 4
ORDER BY "End of Period" DESC, "Last Disbursement Date" DESC
LIMIT 20
```
output
<img src="images/most_recent_pay.png?raw=true"/>

## **Observations and Insights**

This was not a Kaggle dataset, so it required a bit of filtering to clean up some of the results for the more involved queries. As with most real-world data, the data is continuously being updated, so the details of the analysis will change with later updated snapshots from the World Bank.

- The total transactions during this period were $1,140,120.00
- The total transactions per country were (top 5 from 20  from the output - not sorted)

| | country	      |  t_transactions |
| ------------- |:-------------:| -----:|
|1 | Bangladesh       | $42,026  |
|2 | Burkina Faso     |  $22,080 |
|3 | Benin		        |  $17,858 |
|4 | Bolivia	        |  $17,802 |
|5 | Afghanistan	    |  $14,708 |

- The maximum owed to the IDA is $20,713,474,676,596.793
- The borrower that has the most loans is the Ministry of Eco, Planning and RegionDev
- The most recent pay (date) was on 12/28/2022 for the period ending 12/31/2022

---
Thank you for reading!
