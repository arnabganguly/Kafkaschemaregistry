## Deploy a HDInsight Managed Kafka with Confluent Schema Registry 

In this section we would deploy a  HDInsight Managed Kafka  cluster with an Edge Node inside a Virtual Network and then install the Confluent Schema Registry in the Edge Node.  

- Click on the Deploy to Azure Button to start the deployment process

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaschemaregistry%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a><a href="http://armviz.io/#/?load=https://raw.githubusercontent.com/arnabganguly/Kafkaschemaregistry/master/azuredeploy.json" target="_blank">
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

![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaschemaregistry/blob/master/images/Pic2.png)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM3NjY0MTAxOSwtMTk0NjU5ODAwMiwxMj
M5NjI1MDM1LDE2NzQ0MTU0NjNdfQ==
-->