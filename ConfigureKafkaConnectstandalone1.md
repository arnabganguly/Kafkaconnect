
## Configure Kafka Connect in Standalone Mode


### Acquire the Zookeeper and Kafka broker data 

  - Set up password variable. Replace `PASSWORD` with the cluster login password, then enter the command
 ```
 export password='PASSWORD' 
```
   - Extract the correctly cased cluster name

```
export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
```

- Extract the Kafka Zookeeper hosts

```
export KAFKAZKHOSTS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2);
```
- Validate the content of the `KAFKAZKHOSTS` variable
```
echo  $KAFKAZKHOSTS
```
- Zookeeper values appear in the below format . Make a note of these values as they will be used later
```
zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
```

- To extract Kafka Broker information into the variable KAFKABROKERS use the below command

```
export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);
```

- Check to see if the Kafka Broker information is available
```
echo $KAFKABROKERS
```
- Kafka Broker host information appears in the below format
```
wn1-kafka.eahjefyeyyeyeyygqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eaeyhdseyy1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092
```


### Configure Kafka Connect in standalone mode



 - To run Kafka Connect in **standalone mode** one needs to look at two important files. 

  - `connect-standalone.properties` : Located at /usr/hdp/current/kafka-broker/bin

  - `connect-standalone.sh` : Located at /usr/hdp/current/kafka-broker/bin

<br>

> Note : The reason we create two copies of the connect-standalone. properties file below is to separate the rest.port property to different ports. **If you do not do this , you will run into a rest.port conflict** when you try creating the connectors. 

<br>

- Copy the`connect-standalone.properties` to `connect-standalone.properties-1`  and populate the properties as shows below. 

```
sudo cp /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-standalone-1.properties
```
 
   
```
bootstrap.servers=<Enter the full contents of $KAFKABROKERS>
key.converter.schemas.enable=true
value.converter.schemas.enable=true
offset.storage.file.filename=/tmp/connect.offsets1
offset.flush.interval.ms=10000
rest.port=8084
plugin.path=/usr/hdp/current/kafka-broker/connectors/jcustenborder-kafka-connect-twitter-0.3.33,/usr/hdp/current/kafka-broker/connectors/confluentinc-kafka-connect-azure-blob-storage-1.3.2
```

- Copy the `connect-standalone.properties` to ``connect-standalone.properties-2` and edit the properties as below( Note the **changed rest.port** )  

```
sudo cp /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-standalone-2.properties
```
```
bootstrap.servers=<Enter the full contents of $KAFKAZKHOSTS>
key.converter.schemas.enable=true
value.converter.schemas.enable=true
offset.storage.file.filename=/tmp/connect.offsets1
offset.flush.interval.ms=10000
rest.port=8085
plugin.path=/usr/hdp/current/kafka-broker/connectors/jcustenborder-kafka-connect-twitter-0.3.33,/usr/hdp/current/kafka-broker/connectors/confluentinc-kafka-connect-azure-blob-storage-1.3.2
```

### Deploy the Kafka Connect Plugins

- Download the relevant Kafka Plugins from the [Confluent Hub](https://www.confluent.io/hub/) to your local desktop 
     - [Kafka Connect plugin for streaming data from Twitter to Kafka](https://www.confluent.io/hub/jcustenborder/kafka-connect-twitter).
     - [Azure Blob Storage Sink Connector](https://www.confluent.io/hub/confluentinc/kafka-connect-azure-blob-storage)
- Unzip the files to create the folder structures

     ![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic9.png)

  -  Create a new folder path on the edge node and set its properties
  ```
     sudo mkdir /usr/hdp/current/kafka-broker/connectors
     sudo chmod 777 /usr/hdp/current/kafka-broker/connectors 
     ``` 
- Using WINSCP or any other SCP tool of your choice upload the Kafka Connect plugins into the folder path `/usr/hdp/current/kafka-broker/connectors`

![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic11.png)


![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic12.png)


### Configure Kafka Connect plugin for streaming data from Twitter to Kafka 

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
![HDInsight Kafka Connect](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic25.png)


**Update the Kafka Connect plugin for Twitter properties file** 

- Navigate to the folder path `/usr/hdp/current/kafka-broker/connectors` and create a new properties file called `twitter.properties` 
```
cd /usr/hdp/current/kafka-broker/connectors/
sudo vi twitter.properties
```
- Insert the below Twitter Connect plugin properties into the properties file.  
```
"name": "Twitter-to-Kafka",
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
      
### Configure Kafka Connect plugin for Azure Blob Storage Sink connector 

- Create a regular [Azure Blob storage account and a container](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-portal) on Azure and note the storage access keys 

- Navigate to the folder path `/usr/hdp/current/kafka-broker/connectors` and create a new properties file called `blob.properties` 
```
cd /usr/hdp/current/kafka-broker/connectors/
sudo vi blob.properties
```
- Insert the below Azure Blob Storage Sink plugin properties into the properties file
```
name=Kafka-to-Blob
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
 - In the next section we would use the command line to start separate connector instances for running **Source Tasks** and **Sink Tasks**.
 
   Click  [Next ->](https://github.com/arnabganguly/Kafkaconnect/blob/master/ConfigureKafkaConnectstandalone2.md)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY3OTE2Mjg5NCwxNjIzNDU4MTkwLDM0OT
QyNzU1MywxMTUxMjk1NzkyLC0xODA5MzI4MTQ1LC0xMDM5NTIy
OTY4LC02MzYzMDE2NDQsLTg0NzgzODM1NCwtODk5MDI3NzgxXX
0=
-->