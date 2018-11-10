## Kafka 学习笔记
**创建topic**  
`kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test`

**查看topic**  
`kafka-topics  --list --zookeeper localhost:2181`  

**消费者**  
`kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning`  

**生产者**
`kafka-console-producer --broker-list localhost:9092 --topic test`  
