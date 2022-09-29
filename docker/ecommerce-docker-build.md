### run rabbit mq
```
docker run -d --name rabbitmq --network ecommerce-network \
-p 15672:15672 -p 5672:5672 -p 15671:15671 -p 5671:5671 -p 4369:4369 \
-e RABBITMQ_DEFAULT_USER=guest \
-e RABBITMQ_DEFAULT_PASS=guest rabbitmq:management
```


### user-service
```
docker build -t ndy2/user-service:1.0 .
```


### config-server
```
docker build -t ndy2/config-server:1.0 .
```

```
docker run -d -p 8888:8888 --network ecommerce-network \
-e "spring.rabbitmq.host=rabbitmq" \
-e "spring.profiles.active=default" \
--name config-service ndy2/config-server:1.0
```

### service-discovery
```
docker build -t ndy2/service-discovery:1.0 .
```

```
docker run -d -p 8761:8761 --network ecommerce-network \
 -e "spring.cloud.config.uri=http://config-service:8888" \
 --name discovery-service edowon0623/discovery-service:1.0
 ```

 ### api-gateway
 ```
 docker build -t ndy2/api-gateway:1.0 .
 ```

 ```
 docker run -d -p 8000:8000 --network ecommerce-network \
 -e "spring.cloud.config.uri=http://config-service:8888" \
 -e "spring.rabbitmq.host=rabbitmq" \
 -e "eureka.client.serviceUrl.defaultZone=http://service-discovery:8761/eureka/" \
 --name apigateway \
 ndy2/apigateway:1.0
 ```