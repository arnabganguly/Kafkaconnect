## Start source tasks and sink tasks 

- From the edge node run the below to create a new connector and start tasks. 

**Start Source connector** 
```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-1.properties /usr/hdp/current/kafka-broker/connectors/twitter.properties
```
- If the connector is created and tasks are started you will see the below notifications


- Messages ingestion from Twitter will start immediately thereafter 



- One way to test th


**Start Sink Connector**

```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-2.properties /usr/hdp/current/kafka-broker/connectors/blob.properties
```

- If the connector is created and tasks are started you will see the below notifications


- Messages from Twitter will written to the Azure Blob Store 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgxNzUwMDQyMSw3NjQ0MTc1MDYsLTExMz
gwMzE0MDYsNjM0MzAxODM2LDE4OTc3MzAyMDYsMTA3MjUwOTk1
MV19
-->