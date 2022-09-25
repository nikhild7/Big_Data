# Hive Assignment No. 1
## Steps Performed :
### Download vechile sales data -> https://github.com/shashank-mishra219/Hive-Class/blob/main/sales_order_data.csv

-> Data has been downloaded in csv format in Local system and copied to the local system of virtual box with the help of filzilla.

### Store raw data into hdfs location.

-> hadoop fs -ls /tmp/hive_data_class_2/
### Create a internal hive table "sales_order_data_csv" which will store csv data sales_order_data_csv in hive terminal.

-> create table sales_order_data_csv
(
ORDERNUMBER int,
QUANTITYORDERED int,
PRICEEACH float,
ORDERLINENUMBER int,
SALES float,
STATUS string,
QTR_ID int,
MONTH_ID int,
YEAR_ID int,
PRODUCTLINE string,
MSRP int,
PRODUCTCODE string,
PHONE string,
CITY string,
STATE string,
POSTALCODE string,
COUNTRY string,
TERRITORY string,
CONTACTLASTNAME string,
CONTACTFIRSTNAME string,
DEALSIZE string
)
row format delimited
fields terminated by ','
tblproperties("skip.header.line.count"="1");

### Load data from hdfs path into "sales_order_data_csv".

load data inpath '/tmp/hive_data_class_2/sales_order_data.csv' into table sales_order_data_csv;

### Create an internal hive table which will store data in ORC format "sales_order_data_orc".

create table sales_order_data_orc
(
ORDERNUMBER int,
QUANTITYORDERED int,
PRICEEACH decimal(10,2),
ORDERLINENUMBER int,
SALES decimal(10,2),
STATUS string,
QTR_ID int,
MONTH_ID int,
YEAR_ID int,
PRODUCTLINE string,
MSRP int,
PRODUCTCODE string,
PHONE string,
CITY string,
STATE string,
POSTALCODE string,
COUNTRY string,
TERRITORY string ,
CONTACTLASTNAME string,
CONTACTFIRSTNAME string,
DEALSIZE string
)
stored as orc ;

### Load data from "sales_order_data_csv" into "sales_order_data_orc".

->  from sales_order_data_csv insert overwrite table sales_order_data_orc select *;

## Perform below menioned queries on "sales_order_orc" table :

### a. Calculatye total sales per year.

->  select YEAR_ID, sum(SALES) as total_sales from sales_order_data_orc group by YEAR_ID;

![Q1](https://user-images.githubusercontent.com/113916872/191071559-77a3d267-3b8e-490a-89da-8955afd84dbb.png)


### b. Find a product for which maximum orders were placed.
-> select  PRODUCTLINE as maximum_ordered_product,
        sum(QUANTITYORDERED) as total_quantity  
       from sales_order_data_orc 
       group by PRODUCTLINE  
       order by total_quantity desc 
       limit 1;
       ![Q2](https://user-images.githubusercontent.com/113916872/191072388-51c04bc0-850c-406b-ac7c-a7b2e110c594.png)
    
### c. Calculate the total sales for each quarter.

-> select QTR_ID, sum(sales) as total_sales from sales_order_data_orc group by QTR_ID;

![Q3](https://user-images.githubusercontent.com/113916872/191079840-2e15f286-e8eb-4ed4-864c-73f2e193b0b6.png)

### d. In which quarter sales was minimum.

-> select QTR_ID, sum(sales) as total_sales from sales_order_data_orc group by QTR_ID order by total_sales limit 1;

![Q4](https://user-images.githubusercontent.com/113916872/191079997-59b63476-133f-43b2-b22f-d941aae8a5f3.png)

### e. In which country sales was maximum and in which country sales was minimum.

-> select COUNTRY, sum(sales) as total_sales from sales_order_data_orc group by COUNTRY order by total_sales limit 1 UNION all select COUNTRY, sum(sales) as total_sales from sales_order_data_orc group by COUNTRY order by total_sales desc limit 1 ;

![Q5](https://user-images.githubusercontent.com/113916872/191089277-02027588-0b49-4155-85a4-56290f104644.png)


### f. Calculate quartelry sales for each city

-> select  city,CONCAT('Quarter ',qtr_id) as Quarter, sum(sales) as total_sales from  sales_order_data_orc group by city,qtr_id order by city, Quarter;

![Q6](https://user-images.githubusercontent.com/113916872/191089313-4de2f9d4-ce07-4a38-9121-607947f7fba2.png)


### g.  Find a month for each year in which maximum number of quantities were sold.

->  with my as(select year_id, month_id, sum(quantityordered) as total_quantity from sales_order_data_orc group by
    > year_id, month_id), my2 as(select year_id, max(total_quantity) as max_sales from my group by year_id)
    > select my2.year_id, month_id, max_sales from my inner join my2 on my.total_quantity = my2.max_sales;
    
    ![Q7](https://user-images.githubusercontent.com/113916872/192146482-76fca2eb-35bd-4d6e-ab99-c6dc6dd21d43.png)
















