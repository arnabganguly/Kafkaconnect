## Configure Kafka Connect in Distributed Mode

In this section we would configure the Kafka Connect Plugins that we dowloaded earlier .

### Kafka Connect plugin for streaming data from Twitter to Kafka 

**Create a Twitter App and get the credentials** 
 - Go to
   [https://dev.twitter.com/apps/new](https://dev.twitter.com/apps/new
   "https://dev.twitter.com/apps/new") and log in, if necessary
 - Enter your Application Name, Description and your website address. You can leave the callback URL empty.
 - Accept the TOS, and solve the CAPTCHA.
 - Submit the form by clicking the **Create your Twitter Application**
 - Copy the below information from the screen for later use in your properties file.
```
twitter.oauth.consumerKey
twitter.oauth.consumerSecret
twitter.oauth.accessToken
twitter.oauth.accessTokenSecret
```
![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic13.png)


**Update the Kafka Connect plugin for Twitter properties file** 

- Navigate to the 'connectors' folder and create a new properties file called `twitter.properties` 
```
cd /usr/hdp/current/kafka-broker/connectors/
sudo vi twitter.properties
```
- Insert the below Twitter Connect plugin properties into the properties file 
```
"name": "connector1",
"connector.class": "com.github.jcustenborder.kafka.connect.twitter.TwitterSourceConnector",
"tasks.max": 1,
"kafka.status.topic":"twitterstatus",
"kafka.delete.topic":"twitterdelete",        
"topic": "twitter1",   
"twitter.oauth.consumerKey":"<twitter.oauth.consumerKey>",
"twitter.oauth.consumerSecret":"<twitter.oauth.consumerSecret>",
"twitter.oauth.accessToken":"<twitter.oauth.accessToken>",
"twitter.oauth.accessTokenSecret":"<twitter.oauth.accessTokenSecret>",
"filter.keywords":"keyword1,keyword2 ,...",
"process.deletes":false
```
       
  ### Kafka Connect plugin for Azure Blob Storage Sink connector 

- Create a regular [Azure Blob storage account and a container](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) on Azure and note the storage access keys 

- Navigate to the 'connectors' folder and create a new properties file called `blob.properties` 
```
cd /usr/hdp/current/kafka-broker/connectors/
sudo vi blob.properties
```
- Insert the below Azure Blob Storage Sink plugin properties into the properties file
```
name=blob-sink
connector.class=io.confluent.connect.azure.blob.AzureBlobStorageSinkConnector
tasks.max=1
topics=twitterstatus
flush.size=3
azblob.account.name=<Azure Blob account Name>
azblob.account.key=<security key>
azblob.container.name=<container name>
format.class=io.confluent.connect.azure.blob.format.avro.AvroFormat
confluent.topic.bootstrap.servers=<Enter the full contents of $KAFKAZKHOSTS>
confluent.topic.replication.factor=3
```

Click  [Next ->](https://github.com/arnabganguly/Kafkaconnect/blob/master/ConfigureKafkaConnectdistributed1.md)  

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzg5MjA2OTM4XX0=
-->