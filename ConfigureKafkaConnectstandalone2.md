## Start source tasks and sink tasks 

- From the edge node run the below to create a new connector and start tasks. 

**Start Source connector** 
```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-1.properties /usr/hdp/current/kafka-broker/connectors/twitter.properties
```
- If the connector is created and tasks are started you will see the below notifications


- One way to test if Twitter Messages with the keywords are being ingested is to start a console consumer in a different session and start consuming messages from topic *twitterstatus* . 

```
/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKAZKHOSTS --topic twitterstatus 
```
- If everything is working , you should see a stream of relevant Twitter Messages on the console with specified keywords. 


- Messages ingestion from Twitter will start immediately thereafter 





**Start Sink Connector**

```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-2.properties /usr/hdp/current/kafka-broker/connectors/blob.properties
```

- If the connector is created and tasks are started you will see the below notifications


- Messages from Twitter will written to the Azure Blob Store 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA3OTUwOTA4Miw3NjQ0MTc1MDYsLTExMz
gwMzE0MDYsNjM0MzAxODM2LDE4OTc3MzAyMDYsMTA3MjUwOTk1
MV19
-->