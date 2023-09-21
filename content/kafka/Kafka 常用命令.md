# Kafka 常用命令

查看topic列表：
```shell
./kafka-topics.sh --list --bootstrap-server localhost:9092
```

查看某一个消费者组消费了哪些 topic：
```shell
./kafka-consumer-groups.sh --bootstrap-server 8.99.5.109:9092 --group pgcrm-es-index --describe
```

查看某个 topic 的具体情况：
```shell
./kafka-topics.sh --describe --zookeeper 8.99.5.109:2182 --topic es.delta.audit_log
```

