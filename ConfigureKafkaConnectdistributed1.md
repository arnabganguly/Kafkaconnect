## Configure Kafka Connect in Distributed Mode


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


### Configure the nodes for Kafka Connect in Distributed mode


#### Create the topics you will need 

- Create the **Offset Storage** topic with a name of your choice . Here we use *agconnect-offsets*
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agconnect-offsets --zookeeper $KAFKAZKHOSTS
```

- Create the **Config Storage** topic with a name of your choice. Here we use *agconnect-configs* 
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agconnect-configs --zookeeper $KAFKAZKHOSTS
```
- Create the **Status** topic with a name of your choice. Here we use *agconnect-status* 
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agconnect-status --zookeeper $KAFKAZKHOSTS
```

- Create the Topic for storing Twitter Messages. Here we use *twitterstatus*
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic twitterstatus --zookeeper $KAFKAZKHOSTS
```

### Deploy the Kafka Connect Plugins

- Download the relevant Kafka Plugins from the [Confluent Hub](https://www.confluent.io/hub/) to your local desktop 
     - [Kafka Connect plugin for streaming data from Twitter to Kafka](https://www.confluent.io/hub/jcustenborder/kafka-connect-twitter).
     - [Azure Blob Storage Sink Connector](https://www.confluent.io/hub/confluentinc/kafka-connect-azure-blob-storage)
- Unzip the files to create the folder structures

     
     <br />
     <br />
     
     > Note: **The below step needs to be repeated for both ed10 and ed12 edge nodes**
     -  Create a new folder path on the edge node
  ```
     sudo mkdir /usr/hdp/current/kafka-broker/connectors
     sudo chmod 777 /usr/hdp/current/kafka-broker/connectors 
     ``` 
- Using WINSCP or any other SCP tool of your choice upload the Kafka Connect Plugins into folder path created in the last step


- Transfer the files to ed12 using the below command. Make sure that folders have the right permissions for this operation.  
```rsync -r /usr/hdp/current/kafka-broker/connectors/ sshuser@<edge-node12-FQDN>:/usr/hdp/current/kafka-broker/connectors/```

 > Note: **The below steps needs to be repeated for both ed10 and ed12 edge nodes**

#### Configure Kafka Connect 

 - To run Kafka Connect in **distributed mode** one needs to look at two important files. 

  - `connect-distributed.properties` : Located at /usr/hdp/current/kafka-broker/bin/conf

  - `connect-distributed.sh` : Located at /usr/hdp/current/kafka-broker/bin/

    
- In distributed mode, the workers need to be able to discover each other and have shared storage for connector configuration and offset data. Below are some of important parameters we would need to configure. 
    
    - `group.id` : ID that uniquely identifies the cluster these workers belong to. Make sure this value is not changed between the edge nodes.
    -   `config.storage.topic`: Topic to store the connector and task configuration state in.
    -   `offset.storage.topic`: Topic to store the connector offset state in. 
    -   `rest.port`: Port where the REST interface listens for HTTP requests. 
    -  `plugin.path`: Path for the Kafka Connect Plugins 

- Edit the `connect-distributed.properties` file 
``` sudo vi /usr/hdp/current/kafka-broker/conf/connect-distributed.properties ```

- In the  `connect-distributed.properties`  file, define the topics that will store the connector state, task configuration state, and connector offset state. Uncomment and modify the parameters in `connect-distributed.properties`  file as shown below. Note that we use some of the topics we created earlier. 

```
group.id=agconnect-cluster

key.converter=org.apache.kafka.connect.json.JsonConverter
value.converter=org.apache.kafka.connect.json.JsonConverter

key.converter.schemas.enable=true
value.converter.schemas.enable=true

offset.storage.topic=agconnect-offsets
offset.storage.replication.factor=3
offset.storage.partitions=25

config.storage.topic=agconnect-configs
config.storage.replication.factor=3

status.storage.topic=agconnect-status
status.storage.replication.factor=3
status.storage.partitions=5

offset.flush.interval.ms=10000

rest.port=8083

plugin.path=/usr/hdp/current/kafka-broker/connectors/jcustenborder-kafka-connect-twitter-0.3.33,/usr/hdp/current/kafka-broker/connectors/confluentinc-kafka-connect-azure-blob-storage-1.3.2
```

- Start Kafka Connect in distributed mode in the background on the Edge Node . 
    
 ```
 nohup  sudo  /usr/hdp/current/kafka-broker/bin/connect-distributed.sh  /usr/hdp/current/kafka-broker/conf/connect-distributed.properties &
 ```

- Repeat the same steps on other edge node to start Kafka Connect in distributed mode
    
#### Kafka Connect REST API 

> Note : In distributed mode, the REST API is the primary interface to the Connect cluster. Requests can be made from any edge node and the  REST API automatically forwards requests. By default REST API for Kafka Connect runs on port 8083 but is configurable in connector properties

- Use the below REST API calls from any edge node to verify of Kafka Connect is working as expected on both the nodes  

```
curl -s http://<edge-node-FQDN>:8083/ |jq

curl -s http://<edge-node-FQDN>:8083/ |jq
```
- If Kafka Connect is working as expected each of the REST API calls will return an output like below 
```
{
  "version": "2.1.0.3.1.2.1-1",
  "commit": "ded5eefdb4f63651",
  "kafka_cluster_id": "W0HIh8naTgip7Taju7G7fg"
}
 ```
 
 - In this section we started **Kafka Connect in distributed mode** alongside an HDInsight cluster and verified it using the Kafka REST API. 
 
 - In the next section we would use Kafka REST API's to start separate connector instances for running **Source Tasks** and **Sink Tasks**.
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTA4NDI3OTQ2LDgxNDAwODE2NywxNTYzOD
AxNzEsLTE1ODYxMzM5ODAsNDk5MjI2MzEwLDEyNTkxMzIxNDAs
LTk0MjA4MjQ2NF19
-->