# Ad_hoc_analysis_Consumer_Goods_Domain

Codebasics SQL project on Provide Insights to Management in Consumer Goods Domain

Challenge [Link](https://codebasics.io/challenge/codebasics-resume-project-challenge)

### Table of Contents

- [Company Overview And Problem Statement](#company-overview-and-problem-statement)
- [Data Inputs](#data-inputs)
- [Ad hoc Request Along With the Result Visualization and Insights](#ad-hoc-request-along-with-the-result-visualization-and-insights)

### Company Overview And Problem Statement

Atliq Hardwares (imaginary company) is one of the leading computer hardware producers in India and well expanded in other countries too.

However, the management noticed that they do not get enough insights to make quick and smart data-informed decisions. They want to expand their data analytics team by adding several junior data analysts. Tony Sharma, their data analytics director wanted to hire someone who is good at both tech and soft skills. Hence, he decided to conduct a SQL challenge which will help him understand both the skills.

#### Task

- There are 10 ad hoc requests to run a SQL query to answer these requests.
- The target audience of this Project is top-level management – and create a presentation to show the insights.

### Data Inputs

![Data Inputs](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Datasets/ERD_Data_inputs.png)

### Ad hoc Request Along With the Result, Visualization and Insights

- Q1: Provide the list of markets in which customer Atliq Exclusive operates its business in the APAC region

SELECT DISTINCT (market)
 
 FROM dim_customer

WHERE customer="Atliq Exclusive" 
	
 AND region='APAC'

- **Output**

![Q1.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q1_answer.png)

- **Insights**

![Q2.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_1.png)

- It appears that "Atliq Exclusive" operates in 8 markets in the APAC region

- **Q2**: What is the percentage of unique product increase in 2021 vs. 2020? The final output contains these fields.unique_products_2020, unique_products_2021, percentage_chg

with cte20 as
    
    (Select  Count(Distinct(product_code)) As unique_products_2020 
     From fact_sales_monthly
     Where fiscal_year = 2020
     ), 
 
 cte21 as 
  
  (Select  Count(Distinct(product_code)) As unique_products_2021     
   From fact_sales_monthly
     Where fiscal_year = 2021
     ) 

Select cte20.unique_products_2020, cte21.unique_products_2021, 

Round(( unique_products_2021 - unique_products_2020) * 100 / unique_products_2020,2) 

As unique_products_2020unique_products_2021_percentage_chg

From cte20 Cross Join cte21;

- **Output**

![Q2.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q2_answer.png)

- **Insights**

![Q2.Insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_2.png)

- The company added a significant number of new products in 2021 compared to 2020, with a 36.33% increase in the number of products. This could indicate that the company is expanding its product offerings or introducing new products to the market.

- **Q3**:  Provide a report with all the unique product counts for each segment and sort them in descending order of product counts. The final output contains 2 fields, Segment, Product_count.

Select segment, Count(Distinct(product_code)) As product_count

From dim_product

Group By segment

Order By product_count Desc;

- **Output**:

![Q3.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q3_answer.png)

- **Insights**:

![Q3.Insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_3.png)

- Notebook and Accessories are the highest selling categories with 129 and 116 , followed by Peripherals with 84 . Desktop and Storage are the least sold categories with 32 and 27, and Networking has the lowest sales with only 9 units sold. This information can be used to make decisions related to inventory management and marketing strategies.

- **Q4**: Follow-up: Which segment had the most increase in unique products in 2021 vs 2020? The final output contains these fields.Segment, Product_count_2020, Product_count_2021, Difference

with prod_seg as
      
      (SELECT p.segment, COUNT(DISTINCT(s.product_code)) AS product_count, s.fiscal_year 
       FROM dim_product p Join fact_sales_monthly s On p.product_code = s.product_code
       GROUP BY p.segment, s.fiscal_year)

Select prod_2020.segment, prod_2020.product_count As product_count_2020,
       
       prod_2021.product_count As product_count_2021,
      
       prod_2021.product_count - prod_2020.product_count As difference

From prod_seg As prod_2020

Join prod_seg As prod_2021

On prod_2020.segment = prod_2021.segment

And prod_2020.fiscal_year = 2020

And prod_2021.fiscal_year = 2021

Order By difference Desc;

- **Output**:

![Q4.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q4_answer.png)

- **Insights**:

![Q4.insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_4.png)

- we can see that the Accessories segment had the highest increase in products between 2020 and 2021, with an increase of 34 products. The Notebook had the second-highest increase, with the 16 products. The Peripherals, Desktop, Storage, and Networking also experienced increases it’s products, with ranging from 3 to 16.

- **Q5** Get the products that have the highest and lowest manufacturing costs. The final output should contain these fields. Product_code, Product, Manufacturing_cost.

Select p.product_code, p.product, c.manufacturing_cost

From fact_manufacturing_cost c

Join dim_product p On c.product_code = p.product_code

Where c.manufacturing_cost = (Select Max(c.manufacturing_cost) From fact_manufacturing_cost c)

Or c.manufacturing_cost = (Select Min(c.manufacturing_cost) From fact_manufacturing_cost c)

Order By p.product_code Desc;

- **Output**:

![Q5.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q5_answer.png)

- **Insights**:

![Q5.insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_5.png)

- Knowing the manufacturing costs of products is important for businesses to determine the profitability of each product. By comparing the manufacturing cost to the selling price, businesses can determine the profit margin of each product and make informed decisions about pricing and production.

- **Q6**: Generate a report which contains the top 5 customers who received an average high pre_invoice_discount_pct for the fiscal year 2021 and in the Indian market. The final output contains these fields, customer_code, customer, Average_Discount_Percentage

Select c.customer_code, c.customer, Round(Avg(fd.pre_invoice_discount_pct) * 100,2)  As average_discount_percetange

From fact_pre_invoice_deductions fd Join dim_customer c On fd.customer_code = c.customer_code

Where fd.fiscal_year = 2021

And c.market = "India"

Group By c.customer_code

Order By fd.pre_invoice_discount_pct Desc

Limit 5;

- **Output**:

![Q6.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q6_answer.png)

- **Insights**:

![Q6.insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_6.png)

- This report can be useful for understanding which customers in the Indian market are receiving the highest pre-invoice discounts and for identifying potential areas for cost-saving measures.

- **Q7**: Get the complete report of the Gross sales amount for the customer “Atliq Exclusive” for each month. This analysis helps to get an idea of low and high-performing months and take strategic decisions. The final report contains these columns: Month, Year, Gross sales Amount.

Select Monthname(fs.date) As Month, Year(fs.date) As Year, 

Round(Sum(fg.gross_price * fs.sold_quantity) / 1000000,2) As Gross_Sales_Amount

From fact_sales_monthly fs Join dim_customer c On fs.customer_code = c.customer_code

Join fact_gross_price fg On fg.fiscal_year = fs.fiscal_year 

And fg.product_code = fs.product_code

Where c.customer = "Atliq Exclusive"

Group By Month, Year

Order By Gross_Sales_Amount Desc;

- **Output**:

![Q7.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q7_answer.png)

- **Insights**:

![Q7.insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_7.png)

- Atliq Exclusive's best-performing months in terms of gross sales are October, November and December of 2020, with sales of $13.22M $20.46M and $12.94M respectively.

- The lower-performing month is in March, April, May 2020, due to the COVID-19 pandemic

- There seems to be a seasonal trend in Atliq Exclusive's sales, with higher sales during the months of September to December, and lower sales during the months of January to April.

- These insights can help Atliq Exclusive make strategic decisions, such as focusing on marketing and promotions during the months of September to December, and planning for inventory and staffing needs based on the seasonal trends.

- **Q8** In which quarter of 2020, got the maximum total_sold_quantity? The final output contains these fields sorted by the total_sold_quantity, Quarter, Total_Sold_Quantity.

Select case
          
	  When Month(date) In (9, 10, 11) Then "Q1"
          
	  When Month(date) In (12, 1, 2) Then "Q2"
          
	  When Month(date) In (3, 4, 5) Then "Q3"
          
	  Else "Q4"
          
	  End As quarter, Sum(sold_quantity) As total_sold_quantity

From fact_sales_monthly

Where fiscal_year = 2020

Group By quarter

Order By total_sold_quantity Desc;

- **Output**:

![Q8.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q8_answer.png)

- **Insights**:

![Q8.insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_8.png)

- Q1 had the highest sales volume of 7.01M, and Q2 had second-highest sales volume of 6.65M, suggesting that the company's sales and marketing strategies were successful during the beginning of the fiscal year. The lowest sales volume in Q3 2020 is 2.8M, indicating that is the effect of COVID_19 on our sales. In Q4 there is recovery because of high demand on computer accessories.

- **Q9** Which channel helped to bring more gross sales in the fiscal year 2021 and the percentage of contribution? The final output contains these fields, Channel, Gross_Sales_Mln, Percentage

with cte21 as(
      
      Select channel, 
              Round(Sum(fg.gross_price * fs.sold_quantity)/1000000,2) As gross_sales
       From dim_customer c Join fact_sales_monthly fs On c.customer_code = fs.customer_code
       Join fact_gross_price fg On fg.product_code = fs.product_code 
       And fg.fiscal_year = fs.fiscal_year
       Where fs.fiscal_year = 2021
       Group By channel
       Order By gross_sales Desc)

Select *, Concat(Round((gross_sales / Sum(gross_sales) over())*100,2),"%") As percentage

From cte21;

- **Output**:

![Q9.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q9_answer.png)

- **Insights**:

![Q9.insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_9.png)

- The Retailer channel was the top-performing sales channel for the company in fiscal year 2021. this channel performed so well compared to the Direct and Distributor channels.

- **Q10** Get the Top 3 products in each division that have a high total_sold_quantity in the fiscal_year 2021? The final output contains these fields, Division, Product_code, Product, Total_Sold_Quantity, Rank_order

with cte21 As(
			  
     Select p.division, fs.product_code, p.product, Sum(fs.sold_quantity) 
                   As total_sold_quantity,  
              rank() OVER(partition by p.division Order By Sum(fs.sold_quantity)Desc) 
                    As rank_order
              From dim_product p Join fact_sales_monthly fs 
              On p.product_code = fs.product_code
              Where fs.fiscal_year = 2021
              Group By fs.product_code)
              
Select *

From cte21

Where rank_order In (1, 2, 3);

- **Output**;

![Q10.answer](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Q10_answer.png)

- **Insights**:

![Q10.insights](https://github.com/anteportas2023/Ad_hoc_analysis_Consumer_Goods_Domain/blob/main/Insight%20%26%20Output/Insights_10.png)

- The top-selling product in the N & S division is the AQ Pen Drive 2 IN 1, with a total quantity sold of 701,373 units. This is significantly higher than the second and third top-selling products in the same division.
- In the P & A division, the difference in the quantity sold between the top-selling product (AQ Gamers MS) and the second and third top-selling products (AQ Maxima MS) is relatively small, with a difference of only a few hundred units sold.
- The AQ Digit is the top-selling product in the PC division, with a total quantity sold of 17,434 units. However, the difference in quantity sold between the top-selling and second and third top-selling products in this division is relatively small.


