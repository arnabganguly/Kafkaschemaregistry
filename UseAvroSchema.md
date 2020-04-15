
## Send and consume Avro data from Kafka using schema registry

- Create a fresh Kafka Topic ```agkafkaschemareg```
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agkafkaschemareg --zookeeper $KAFKAZKHOSTS
```

- Use the **Kafka Avro Console Producer** to create a schema , assign the schema to the Topic and start sending data to the topic in Avro format. Ensure that the Kafka Topic in the previous step is successfully created and that **$KAFKABROKERS** has a value in it. 

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
![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaschemaregistry/blob/master/images/Pic7.png)
- Try entering random non schema data into the console producer to see how the producer does now allow any data that does not conform to predefined Avro schema. 
```
1       {"cust_id":1313131, "year":2012, "expenses":[1313.13, 2424.24]}
2       {"cust_id":1313131,"cust_age":34 "year":2012, "expenses":[1313.13, 2424.24,34.212]}
org.apache.kafka.common.errors.SerializationException: Error deserializing json {"cust_id":1313131,"cust_age":34 "year":2012, "expenses":[1313.13, 2424.24,34.212]} to Avro of schema {"type":"record","name":"example_schema","namespace":"com.example","fields":[{"name":"cust_id","type":"int","doc":"Id of the customer account"},{"name":"year","type":"int","doc":"year of expense"},{"name":"expenses","type":{"type":"array","items":"float"},"doc":"Expenses for the year"}],"doc:":"A basic schema for storing messages"}
Caused by: org.codehaus.jackson.JsonParseException: Unexpected character ('"' (code 34)): was expecting comma to separate OBJECT entries
 at [Source: java.io.StringReader@3561c410; line: 1, column: 35]
        at org.codehaus.jackson.JsonParser._constructError(JsonParser.java:1433)
        at org.codehaus.jackson.impl.JsonParserMinimalBase._reportError(JsonParserMinimalBase.java:521)
        at org.codehaus.jackson.impl.JsonParserMinimalBase._reportUnexpectedChar(JsonParserMinimalBase.java:442)
        at org.codehaus.jackson.impl.ReaderBasedParser.nextToken(ReaderBasedParser.java:406)
        at org.apache.avro.io.JsonDecoder.getVaueAsTree(JsonDecoder.java:549)
        at org.apache.avro.io.JsonDecoder.doAction(JsonDecoder.java:474)
        at org.apache.avro.io.parsing.Parser.advance(Parser.java:88)
        at org.apache.avro.io.JsonDecoder.advance(JsonDecoder.java:139)
        at org.apache.avro.io.JsonDecoder.readInt(JsonDecoder.java:166)
        at org.apache.avro.io.ValidatingDecoder.readInt(ValidatingDecoder.java:83)
        at org.apache.avro.generic.GenericDatumReader.readInt(GenericDatumReader.java:511)
        at org.apache.avro.generic.GenericDatumReader.readWithoutConversion(GenericDatumReader.java:182)
        at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:152)
        at org.apache.avro.generic.GenericDatumReader.readField(GenericDatumReader.java:240)
        at org.apache.avro.generic.GenericDatumReader.readRecord(GenericDatumReader.java:230)
        at org.apache.avro.generic.GenericDatumReader.readWithoutConversion(GenericDatumReader.java:174)
        at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:152)
        at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:144)
        at io.confluent.kafka.formatter.AvroMessageReader.jsonToAvro(AvroMessageReader.java:213)
        at io.confluent.kafka.formatter.AvroMessageReader.readMessage(AvroMessageReader.java:200)
        at kafka.tools.ConsoleProducer$.main(ConsoleProducer.scala:59)
        at kafka.tools.ConsoleProducer.main(ConsoleProducer.scala)

```

- In a different screen start the Kafka Avro Console Consumer

```
sudo /usr/bin/kafka-avro-console-consumer --bootstrap-server $KAFKABROKERS --topic agkafkaschemareg --from-beginning
```

- You should start seeing the below output

```
{"cust_id":1313131,"year":2012,"expenses":[1313.13,2424.24]}
{"cust_id":7979797,"year":2011,"expenses":[4489.0]}
{"cust_id":3535353,"year":2011,"expenses":[761.35,92.18,14.41]}
```
![HDInsight Kafka Schema Registry](https://github.com/arnabganguly/Kafkaschemaregistry/blob/master/images/Pic8.png)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5NTQ5MDY4NTQsMTg3ODEyMTM5Niw3MT
g0NTM2ODUsNjA3NTY1OTMwXX0=
-->