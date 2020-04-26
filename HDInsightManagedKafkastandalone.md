## Deploy a HDInsight Managed Kafka with a Kafka connect standalone

In this section we would deploy an HDInsight Managed Kafka  cluster with two edge nodes inside a Virtual Network and then enable Kafka Connect in standalone mode on one of those edge nodes.  

- Click on the Deploy to Azure Button to start the deployment process

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaconnect%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a><a href="http://armviz.io/#/?load=https://raw.githubusercontent.com/arnabganguly/Kafkaconnect/master/azuredeploy.json" target="_blank">
  <img src="http://armviz.io/visualizebutton.png"/>
</a>

</br>
</br>

 - On the Custom deployment template populate the fields as described below. Leave the rest of their fields at their default entries
    -  **Resource Group** : Choose a previously created resource group from the dropdown
    - **Location** : Automatically populated based on the Resource Group location 
    - **Cluster Name** : Enter a cluster name( or one is created by default)
    - **Cluster Login Name**: Create a administrator name for the Kafka Cluster( example : admin) 
    - **Cluster Login Password**: Create a administrator login password for the username chosen above
    - **SSH User Name**: Create an SSH username for the cluster
    - **SSH Password**: Create an SSH password for the username chosen above

- Check he box titled "*I agree to the terms and conditions stated above*" and click on **Purchase**. 
    
![HDInsight Kafka Schema Registry]()

- Wait till the deployment completes and you get the *Your Deployment is Complete* message and then click on  **Go to resource**.

![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic3.png)



- On the Resource group explore the various components created as part of the Deployment . Click on the HDInsight Cluster to open the cluster page. 

![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic5.png)

- On the HDInsight cluster page click on the SSH+Cluster login blade on the left and get the hostname of the edge node that was deployed. 

![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaconnect/blob/master/images/pic10.png)

- Using an SSH client of your choice ssh into the edge node using the **sshuser** and **password** that you set in the custom ARM script. 

> Note:  In this the Kafka Connect standalone mode you will need to make config changes on a single edge node . 

- In the next sections we would configure the Kafka Connect  standalone on a single edge node.  

Click  [Next ->](https://github.com/arnabganguly/Kafkaconnect/blob/master/ConfigureKafkaConnectstandalone1.md)  
                                                  
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA2MTY4MTAzNCwxNzkyMjk4Mzk5LC0yMT
Q0NTA1ODQzLDMwNjA5NzQ4MywxMTYwMTg4NTg4XX0=
-->