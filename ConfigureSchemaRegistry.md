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
- Replace the kafastore.connection.url variable with the Zookeeper string that you noted earlier.  Also replace the debug variable to true . If set to true true, API requests that fail will include extra debugging information, including stack traces. The properties files now looks like this.  

```
listeners=http://0.0.0.0:8081
kafkastore.connection.url=zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
kafkastore.topic=_schemas
debug=true
```

- Save and exit the properties file using ``` :wq``` command

- Use the below commands to **Start the Schema Registry** and point it to use the updated Schema Registry properties file
```
cd /bin
``` 

 ```
 $ sudo schema-registry-start /etc/schema-registry/schema-registry.properties
 ```


- Schema Registry Starts and starts listening for requests. 
```
...
...
[2020-03-22 13:24:49,089] INFO Adding listener: http://0.0.0.0:8081 (io.confluent.rest.Application:190)
[2020-03-22 13:24:49,154] INFO jetty-9.2.24.v20180105 (org.eclipse.jetty.server.Server:327)
[2020-03-22 13:24:49,753] INFO HV000001: Hibernate Validator 5.1.3.Final (org.hibernate.validator.internal.util.Version:27)
[2020-03-22 13:24:49,902] INFO Started o.e.j.s.ServletContextHandler@40844aab{/,null,AVAILABLE} (org.eclipse.jetty.server.handler.ContextHandler:744)
[2020-03-22 13:24:49,914] INFO Started NetworkTrafficServerConnector@33fe57a9{HTTP/1.1}{0.0.0.0:8081} (org.eclipse.jetty.server.NetworkTrafficServerConnector:266)
[2020-03-22 13:24:49,915] INFO Started @2780ms (org.eclipse.jetty.server.Server:379)
[2020-03-22 13:24:49,915] INFO Server started, listening for requests... (io.confluent.kafka.schemaregistry.rest.SchemaRegistryMain:45)
```

- With the Schema Registry running in one Launch another SSH window

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzk4MzE3MzUxLDE0NDQxNDc3OTcsMTcyOT
c3NzM4NywtMTczMDE0OTg0NSwtMTY5NjMxMTY2N119
-->