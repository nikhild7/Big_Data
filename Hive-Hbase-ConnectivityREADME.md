# Hive-Hbase Connectivity with sample database 

## Dataset used for given task : https://drive.google.com/file/d/1MTiqMHh7V7Smu7IRYr4pBtHcVZZNLixy/view?usp=sharing

## Create a Column Family in Hbase using following command:
=>create'Sales_order_hbase_csv','ORDERNUMBER','QUANTITYORDERED','PRICEEACH','ORDERLINENUMBER','SALES','STATUS','QTR_ID','MONTH_ID','YEAR_ID','PRODUCTLINE','MSRP','CODE','PHONE','CITY','STATE','POSTALCODE','COUNTRY','TERRITORY','CONTACTLASTNAME','CONTACTFIRSTNAME','DEALSIZE'

## Create a Hive-stage table :

=> create EXTERNAL table sales_order_csv_new
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
row format delimited
fields terminated by ',' 
tblproperties("skip.header.line.count" = "1");

![image](https://user-images.githubusercontent.com/113916872/200172601-6905d9a3-62a8-4019-a6f2-a777125b82b9.png)

## Hbase Storage handler property :
=> The storage handler is built as an independent module, hive-hbase-handler-x.y.z.jar, which must be available on the Hive client auxpath, along with HBase, Guava and ZooKeeper jars. It also requires the correct configuration property to be set in order to connect to the right HBase master. 
Storage Handlers are a combination of InputFormat , OutputFormat , SerDe , and specific code that Hive uses to treat an external entity as a standard Hive table.

  'org.apache.hadoop.hive.hbase.HBaseStorageHandler'


## If you want to give Hive access to an existing HBase table, use CREATE EXTERNAL TABLE using Hbase storage handler property with columns mapping :

=>CREATE EXTERNAL TABLE IF not exists sales_order_csv_main
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
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
WITH SERDEPROPERTIES
("hbase.columns.mapping"=":key,QUANTITYORDERED:QUANTITYORDERED,
PRICEEACH:PRICEEACH,ORDERLINENUMBER:ORDERLINENUMBER,SALES:SALES,STATUS:STATUS,QTR_ID:QTR_ID,MONTH_ID:MONTH_ID,
YEAR_ID:YEAR_ID,PRODUCTLINE:PRODUCTLINE,MSRP:MSRP,PRODUCTCODE:PRODUCTCODE,PHONE:PHONE,CITY:CITY,STATE:STATE,POSTALCODE:POSTALCODE,COUNTRY:COUNTRY,TERRITORY:TERRITORY,
CONTACTLASTNAME:CONTACTLASTNAME,CONTACTFIRSTNAME:CONTACTFIRSTNAME,DEALSIZE:DEALSIZE")
TBLPROPERTIES("hbase.table.name"="Sales_order_hbase_csv")

![image](https://user-images.githubusercontent.com/113916872/200172913-b8baf7ef-82bc-4a6f-b103-5b12f38a7e58.png)

## Load the data into a stage table :

=> Load data local inpath ‘/home/cloudera/sales_order_data_new.csv’ into table sales_order_csv_new;

## Load data into Hive main table from Hive stage table:

=>insert into table sales_order_csv_main select * from sales_order_csv_new;

![image](https://user-images.githubusercontent.com/113916872/200172719-2aaf2926-058b-4128-8b13-d484a9be7500.png)

![image](https://user-images.githubusercontent.com/113916872/200172910-34d3d76d-2138-4786-97fd-afc022e92267.png)

## From the HBase shell, verify that the data got loaded:

=> Scan 'Sales_order_hbase_csv' 

![image](https://user-images.githubusercontent.com/113916872/200173123-6f6493d2-f49f-4149-94c4-b62bef995bc8.png)











