## Start source tasks and sink tasks 

- From the edge node  run each of the below in a **separate session** to create new connectors and start tasks.  

**Start Source connector** 
- In a new session start the source connector

```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-1.properties /usr/hdp/current/kafka-broker/connectors/twitter.properties
```
- If the connector is created and tasks are started you will see the below notifications

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic21.png)


- Message ingestion from Twitter will start immediately thereafter. 

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic22.png)

- One other to way to test if Twitter Messages with the keywords are indeed being ingested is to start a console consumer in a fresh session and start consuming messages from topic *twitterstatus* .In a new session , launch a console consumer. (Make sure *$KAFKAZKHOSTS* still holds values of Kafka brokers)

```
/usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKAZKHOSTS --topic twitterstatus 
```
- If everything is working , you should see a stream of relevant Twitter Messages on the console with specified keywords. 


**Start Sink Connector**

- In a new session start the sink connector
```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-2.properties /usr/hdp/current/kafka-broker/connectors/blob.properties
```

- If the connector is created and tasks are started you will see the below notifications

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic23.png)

- Messages from the Kafka Topic *twitterstatus* will be written to container on the Azure Blob Store 

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic24.png)

- Authenticate into your Azure portal and navigate to the storage account to validate if Twitter Messages are being sent to the specific container. 

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic19.png)

 ![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic20.png)



- This ends the lab for Kafka Connect in standalone mode. 

<!--stackedit_data:
eyJoaXN0b3J5IjpbNDEyMTYzOTE4LC0zNjM0NzkyNjcsLTY2NT
Q3NTkwMywxOTQ5MjMzNzI0LC0xODgwNDgwOTU5LDc2NDQxNzUw
NiwtMTEzODAzMTQwNiw2MzQzMDE4MzYsMTg5NzczMDIwNiwxMD
cyNTA5OTUxXX0=
-->