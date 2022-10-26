# This is a real time dataset of the ineuron technical consultant team. You have to perform hive analysis on this given dataset.

Download Dataset 1 - https://drive.google.com/file/d/1WrG-9qv6atP-W3P_-gYln1hHyFKRKMHP/view

Download Dataset 2 - https://drive.google.com/file/d/1-JIPCZ34dyN6k9CqJa-Y8yxIGq6vTVXU/view

##1. Create a schema based on the given dataset.
=> A) 
Create table AgentPerformance
(
sr_no int,
Date date,
Agent_Name string,
Total_charts string,
Avg_Response_Time string,
Avg_Resolution_Time string,
Avg_Rating float,
Total_Feedback int
)
row format delimited
fields terminated by ','
tblproperties ("skip.header.line.count" = "1");
B) 
Hive> Create table AgentLogingReport
(
sr_no int,
Agent string,
Date date,
Login string,
Logout string,
Duration string
)
row format delimited
fields terminated by ','
tblproperties ("skip.header.line.count" = "1");

##2.Dump the data inside the hdfs in the given schema location.
=> A)load data local inpath ‘/home/cloudera/AgentLogingReport.csv’ into table AgentLogingReport;
B) load data local inpath ‘/home/cloudera/AgentPerformance.csv’ into table 
AgentPerformance;

##3. List of all agents' names. 
![image](https://user-images.githubusercontent.com/113916872/198081684-dd97d7b2-d915-46d1-8535-409d354e39be.png)




