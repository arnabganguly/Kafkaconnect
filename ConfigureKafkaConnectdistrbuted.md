## Configure the Confluent Schema Registry

To run the HDInsight worker in distributed mode one needs to look at two important files 

- `connect-distributed.properties` : Located at /usr/hdp/current/kafka-broker/bin/

- `connect-distributed.sh` : Located at /usr/hdp/current/kafka-broker/conf/


2.  In the  `connect-distributed.properties`  file, define the topics that will store the connector state, task configuration state, and connector offset state.

   In distributed mode, the workers need to be able to discover each other and have shared storage for connector configuration and offset data. In addition to the usual worker settings, ensure you have configured the following for the cluster:
    
    -   group.idID that uniquely identifies the cluster these workers belong to. Ensure this is unique for all groups that work with a cluster.
    -   config.storage.topic  - Topic to store the connector and task configuration state in.
    -   offset.storage.topic  - Topic to store the connector offset state in. 
    -   rest.port  - Port where the REST interface listens for HTTP requests. 






3.  Set the group.id value for all of the workers in the cluster.
    
    Note:  All workers that belong to the same cluster must have the same group.id value.
    
4.  Run the distributed worker command, connect-distributed.sh.
    
    For example:
    
    ```
    cd /opt/mapr/kafka/kafka-1.0/
    ./bin/connect-distributed.sh 
    ./config/connect-distributed.properties
    ```
    

Note:  >Distributed mode does not have any additional command line parameters. If other instances are already running, new workers either start a new group or join an existing one, and then wait for work to do. For information on managing the connectors running in the cluster, see  [REST API](https://mapr.com/docs/60/Kafka/Connect-rest-api.html "The Kafka Connect REST API for MapR-ES manages connectors.").
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNTgwNTExNzUsMTgyMzE4MDcxNiwtMT
A3NDM1MjM1NywtMTU3MTA5MTcxOV19
-->