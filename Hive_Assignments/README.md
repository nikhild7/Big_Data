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





