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

- Create a regular Blob storage account on Azure and note the storage access keys 

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
confluent.topic.bootstrap.servers=<Enter contents of $KAFKAHOSTS>
confluent.topic.replication.factor=3
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzA4OTMyODExLDE0ODU5MTM5MTQsLTUyMT
Q4MzIzOCwyMjYzMzgxNTgsMTE2NTY1ODIxNiw2NTk4NDc4Mjld
fQ==
-->