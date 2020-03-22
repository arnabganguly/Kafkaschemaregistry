## Configure the Confluent Schema Registry

The confluent schema registry is located at  ``` /etc/schema-registry/schema-registry.properties ``` and the mechanisms to start and stop service executables are located at the  ```/usr/bin/``` folder. 

The Schema Register needs to know the Zookeeper service to be able to interact with HDInsight Kafka cluster. Follow the below steps to get the details of the Zookeeper Quorum.

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
- Validate the content of the ```KAFKAZKHOSTS``` variable
```
echo  $KAFKAZKHOSTS
```
- Make a note of the values in the 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MzAxNDk4NDUsLTE2OTYzMTE2NjddfQ
==
-->