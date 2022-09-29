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

```
docker run -d --network ecommerce-network \
  --name user-service \
 -e "spring.cloud.config.uri=http://config-service:8888" \
 -e "spring.rabbitmq.host=rabbitmq" \
 -e "spring.zipkin.base-url=http://zipkin:9411" \
 -e "eureka.client.serviceUrl.defaultZone=http://discovery-service:8761/eureka/" \
 -e "logging.file=/api-logs/users-ws.log" \
 edowon0623/user-service
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

### order-service
```
docker run -d --network ecommerce-network \
  --name order-service \
 -e "spring.zipkin.base-url=http://zipkin:9411" \
 -e "eureka.client.serviceUrl.defaultZone=http://discovery-service:8761/eureka/" \
 -e "spring.datasource.url=jdbc:mariadb://mariadb:3306/mydb" \
 -e "logging.file=/api-logs/orders-ws.log" \
 ndy2/order-service
 ```

### product-service
docker run -d --network ecommerce-network \
  --name product-service \
 -e "eureka.client.serviceUrl.defaultZone=http://discovery-service:8761/eureka/" \
 -e "logging.file=/api-logs/product-ws.log" \
 ndy2/product-service
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
 docker build -t ndy2/apigateway:1.0 .
 ```

 ```
 docker run -d -p 8000:8000 --network ecommerce-network \
 -e "spring.cloud.config.uri=http://config-service:8888" \
 -e "spring.rabbitmq.host=rabbitmq" \
 -e "eureka.client.serviceUrl.defaultZone=http://service-discovery:8761/eureka/" \
 --name apigateway \
 ndy2/apigateway:1.0
 ```

 ### maria db
Dockerfile
```
FROM mariadb

ENV MYSQL_ROOT_PASSWORD test1357
ENV MYSQL_DATABASE mydb

COPY ./mysql_data/mysql /var/lib/mysql

EXPOSE 3306

ENTRYPOINT ["mysqld", "--user=root"]
```
Docker 실행
```
docker run -d -p 3306:3306  --network ecommerce-network --name mariadb ndy2/my-mariadb:1.0
```


### zipkin
```
docker run -d -p 9411:9411 \
 --network ecommerce-network \
 --name zipkin \
 openzipkin/zipkin 
```

### Prometheus
```
docker run -d -p 9090:9090 \
 --network ecommerce-network \
 --name prometheus \
 -v /Users/dowonlee/Desktop/Work/springcloud/prometheus-2.25.0.darwin-amd64/prometheus.yml:/etc/prometheus/prometheus.yml \
 prom/prometheus 
```

### Grafana
```
docker run -d -p 3000:3000 \
 --network ecommerce-network \
 --name grafana \
 grafana/grafana 
 ```
 

### docker build all
docker build -t ndy2/user-service:1.0 user-service/.
docker build -t ndy2/order-service:1.0 order-service/.
docker build -t ndy2/product-service:1.0 product-service/.
docker build -t ndy2/service-discovery:1.0 service-discovery/.
docker build -t ndy2/apigateway:1.0 api-gateway/.
docker build -t ndy2/config-server:1.0 config-server/.