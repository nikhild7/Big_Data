# This is a real time dataset of the ineuron technical consultant team. You have to perform hive analysis on this given dataset.

Download Dataset 1 - https://drive.google.com/file/d/1WrG-9qv6atP-W3P_-gYln1hHyFKRKMHP/view

Download Dataset 2 - https://drive.google.com/file/d/1-JIPCZ34dyN6k9CqJa-Y8yxIGq6vTVXU/view

## 1.Create a schema based on the given dataset.
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

## 2.Dump the data inside the hdfs in the given schema location.
=> A)load data local inpath ‘/home/cloudera/AgentLogingReport.csv’ into table AgentLogingReport;
B) load data local inpath ‘/home/cloudera/AgentPerformance.csv’ into table 
AgentPerformance;

## 3. List of all agents' names. 

=> Hive>  select distinct Agent_Name from AgentPerformance;

![image](https://user-images.githubusercontent.com/113916872/198081684-dd97d7b2-d915-46d1-8535-409d354e39be.png)

## 4. Find out agent average rating.
=> select Agent_name,avg(Avg_Rating) from AgentPerformance group by Agent_name;
![image](https://user-images.githubusercontent.com/113916872/198083631-37190c30-24f6-439f-beed-a5a17c233c36.png).

## 5 Total working days for each agents
=>select Agent,count(distinct Date) from AgentLogingReport group by Agent;
![image](https://user-images.githubusercontent.com/113916872/198819409-c5d2f4fb-1a07-4b08-aae9-137223ee30c0.png)


## 6. Total query that each agent have taken
=> Hive> select Agent_name,sum(total_chats) from AgentPerformance group by Agent_name;

![image](https://user-images.githubusercontent.com/113916872/198106279-9df2c686-134a-4960-8ef3-ebdaebcb4fb1.png)

## 7. Total Feedback that each agent have received 
=> select Agent_name, sum(Total_feedback) from agentperformance group by Agent_name;

![image](https://user-images.githubusercontent.com/113916872/198819727-baa7c850-36c4-43f6-9432-0e6d28e32551.png)

## 8.Agent name who have average rating between 3.5 to 4 

=>select Agent_name, Avg_Rating from agentperformance where Avg_Rating between 3.5 and 4;

![image](https://user-images.githubusercontent.com/113916872/198819935-542d0057-9341-4121-bb6d-276994f1f61c.png)

## 9. Agent name who have rating less than 3.5 
=> select Agent_name, Avg_Rating from agentperformance where Avg_Rating < 3.5;
![image](https://user-images.githubusercontent.com/113916872/198820030-b62d3129-c4cf-44cf-b356-77710159dfcf.png)

## 10. Agent name who have rating more than 4.5 
=> Hive> select Agent_name,Avg_Rating from AgentPerformance where Avg_Rating > 4.5;
![image](https://user-images.githubusercontent.com/113916872/198820070-1600994a-7072-4162-b0a6-2a2406f4b437.png)

## 14. Find the number of chat on which they have received a feedback 
=>select agent_name ,total_feedback , sum( total_charts) as No_charts  from agentperformance where total_feedback <> 0 group by agent_name, Total_feedback  limit 10

![image](https://user-images.githubusercontent.com/113916872/198823556-43707bd4-f874-4a8a-839a-fe277b91074c.png)












