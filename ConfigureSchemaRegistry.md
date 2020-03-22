## Configure the Confluent Schema Registry

The confluent schema registry is located at  ``` /etc/schema-registry/schema-registry.properties ``` and the mechanisms to start and stop service executables are located at the  ```/usr/bin/``` folder. 

The Schema Register needs to know the Zookeeper service to be able to interact with HDInsight Kafka cluster. Follow the below steps to get the details of the Zookeeper Quorum.

 - Set up password variable. Replace `PASSWORD` with the cluster login password, then enter the command

```export password='PASSWORD' ```

- Extract the correctly cased cluster name
``` 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU5NjQ0MzI1XX0=
-->