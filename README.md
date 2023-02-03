# Kafka - Projects Udemy Course

Projects from the Udemy course [Apache Kafka](https://www.udemy.com/course/apache-kafka-valdir)

## Projects

- [str-producer](./str-producer/)
- [str-consumer](./str-consumer/)
- [payment-service](./payment-service/)
- [json-consumer](./json-consumer/)

### Getting Started

``` bash
docker-compose up -d
```

- [Kafka Drop - http://localhost:19000/](http://localhost:19000/)
- [Str Producer - http://localhost:8000/](http://localhost:8000/)

## Docker

- Build image from [Docekrfile](./payment-service/Dockerfile)

``` bash
 docker build -t jjeanjacques10/payment-service:1.0.0 .
```

## Notes

- When it configs the same groupId for different @KafkaListener, they will listen to different partitions.

``` bash
group-1: partitions assigned: [str-topic-1]
group-1: partitions assigned: [str-topic-0]
group-2: partitions assigned: [str-topic-0, str-topic-1] # group-2 will listen to both partitions
```

- Create a custom KafkaListener

``` java
// Before
@KafkaListener(groupId = "group-0", topicPartitions = {
        @TopicPartition(topic = "str-topic", partitions = {"0"})
}, containerFactory = "strContainerFactory")
public void create(String message) {...}

// After
@StrConsumerCustomListener(groupId = "group-0")
public void create(String message) {...}
```

- Add interceptor to Consumer

``` java
factory.setRecordInterceptor(validMessage());
```

- Create a Json Consumer

``` java
  @Bean
    public ConcurrentKafkaListenerContainerFactory<String, Object> jsonContainerFactory(
            ConsumerFactory<String, Object> jsonConsumerFactory
    ) {
        var factory = new ConcurrentKafkaListenerContainerFactory<String, Object>();
        factory.setConsumerFactory(jsonConsumerFactory);
        factory.setMessageConverter(new JsonMessageConverter()); // <--- Add this line
        return factory;
    }
```

---
Developed by [Jean Jacques Barros](https://github.com/jjeanjacques10/)
