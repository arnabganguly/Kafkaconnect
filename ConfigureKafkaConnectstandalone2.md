## Start source tasks and sink tasks 

- From the edge node run the below to create a new connector and start tasks. 

**Start Source connector** 
```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-1.properties /usr/hdp/current/kafka-broker/connectors/twitter.properties
```




**Start Sink Connector**

```
sudo /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone-2.properties /usr/hdp/current/kafka-broker/connectors/blob.properties
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzY0NDE3NTA2LC0xMTM4MDMxNDA2LDYzND
MwMTgzNiwxODk3NzMwMjA2LDEwNzI1MDk5NTFdfQ==
-->