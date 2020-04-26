## Start source tasks and sink tasks 

- From the edge node run the below to create a new connector and start tasks. 

```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-1.properties /usr/hdp/current/kafka-broker/connectors/twitter.properties
```






```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-2.properties /usr/hdp/current/kafka-broker/connectors/blob.properties
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMzgwMzE0MDYsNjM0MzAxODM2LDE4OT
c3MzAyMDYsMTA3MjUwOTk1MV19
-->