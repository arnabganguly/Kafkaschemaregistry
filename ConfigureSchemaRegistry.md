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
- Zookeeper values appear in the below format . Make a note of these values as they will be used later
```
zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
```

- Open the Schema Registry properties files in edit mode

``` 
sudo vi /etc/schema-registry/schema-registry.properties
```
- By default the file would contain the below parameters 
```
listeners=http://0.0.0.0:8081
kafkastore.connection.url=zk0-ohkl-h:2181,zk1-ohkl-h:2181,zk2-ohkl-h:2181
kafkastore.topic=_schemas
debug=false
```
- Replace the kafastore.connection.url variable with the Zookeeper string that you noted earlier. The properties files now looks like this.  

```
listeners=http://0.0.0.0:8081
kafkastore.connection.url=zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
kafkastore.topic=_schemas
debug=false
```

- Save and exit the properties file using ``` :wq``` command

- Use the below commands to start the Schema Registry and point it to use the updated Schema Registry properties file
```
cd /bin
``` 

 ```
 $ bin/schema-registry-start etc/schema-registry/schema-registry.properties
 ```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMjc0NzYyNjEsLTE3MzAxNDk4NDUsLT
E2OTYzMTE2NjddfQ==
-->