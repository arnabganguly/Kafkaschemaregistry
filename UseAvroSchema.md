
## Send and consume Avro data from Kafka using schema registry

- Create a fresh Kafka Topic ```agkafkaschemareg```
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agkafkaschemareg --zookeeper $KAFKAZKHOSTS
```

- Use the **Kafka Avro Console Producer** to create a schema , assign the schema to the Topic and start sending data to the topic in Avro format. 

- The schema we are sending is a Key: Value Pair
```
Key : Int 
```


```
Value
{
  "type": "record",
  "name": "example_schema",
  "namespace": "com.example",
  "fields": [
    {
      "name": "cust_id",
      "type": "int",
      "doc": "Id of the customer account"
    },
    {
      "name": "year",
      "type": "int",
      "doc": "year of expense"
    },
    {
      "name": "expenses",
      "type": {
        "type": "array",
        "items": "float"
      },
      "doc": "Expenses for the year"
    }
  ],
  "doc:": "A basic schema for storing messages"
} 

 
```
- Use the below command to start the **Kafka Avro Console Producer**
``` 
/usr/bin/kafka-avro-console-producer     --broker-list $KAFKABROKERS     --topic agkafkaschemareg     --property parse.key=true --property key.schema='{"type" : "int", "name" : "id"}'     --property value.schema='{ "type" : "record", "name" : "example_schema", "namespace" : "com.example", "fields" : [ { "name" : "cust_id", "type" : "int", "doc" : "Id of the customer account" }, { "name" : "year", "type" : "int", "doc" : "year of expense" }, { "name" : "expenses", "type" : {"type": "array", "items": "float"}, "doc" : "Expenses for the year" } ], "doc:" : "A basic schema for storing messages" }'
```

- When the producer is ready to accept messages start sending the messages in the predefined Avro schema format. Use the Tab key to create spacing between the Key and Value. 
```
1 TAB {"cust_id":1313131, "year":2012, "expenses":[1313.13, 2424.24]}
2 TAB {"cust_id":3535353, "year":2011, "expenses":[761.35, 92.18, 14.41]}
3 TAB {"cust_id":7979797, "year":2011, "expenses":[4489.00]}
```
- Try entering random non schema data into the console producer to see how the producer does now allow any data that does not conform to predefined Avro schema. 

- In a different screen start the Kafka Avro Console Consumer

```
sudo /usr/bin/kafka-avro-console-consumer --bootstrap-server $KAFKABROKERS --topic agkafkaschemareg --from-beginning
```

- You should start seeing the below output


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1MzM2NzQ4MzMsNzE4NDUzNjg1LDYwNz
U2NTkzMF19
-->