
## Send and consume Avro data from Kafka using schema registry

- Create a fresh Kafka Topic ```agkafkaschemareg```
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agkafkaschemareg --zookeeper $KAFKAZKHOSTS
```

- Use the Kafka Avro Console Producer to create a schema , assign the schema to the Topic and 
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTg5OTQ0MTY1XX0=
-->