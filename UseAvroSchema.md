
## Send and consume Avro data from Kafka using schema registry

- Create a fresh Kafka Topic
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc5Mzc4OTE4OV19
-->