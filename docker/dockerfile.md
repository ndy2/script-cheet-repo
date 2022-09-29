
초간단 도커 파일 예시
```dockerfile
FROM openjdk:8-jdk-alphin

VOLUME /tmp

# maven
#COPY target/user-ws-0.1.jar users-service.jar 
#gradle
COPY build/libs/user-0.0.1-SNAPSHOT.jar UserService.jar

ENTRYPOINT [
"java", 
"-Djava.security.egd=file:/dev/.urandom",
"-jar",
"users-service.jar"
]
```