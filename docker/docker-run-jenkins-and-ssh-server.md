
### jenkins 
```
docker run -d 
-p 8080:8080 -p 50000:50000 
--name jenkins-server 
--restart=on-failure 
-v jenkins_home:/var/jenkins_home/jenkins/jenkins:lts-jdk11
```



### MacOS apple silicon chip, m1) SSH 서버 (with 도커) 실행 명령어
```
docker run 
--privileged 
--name docker-server -itd 
-p 10022:22 -p 8081:8080 
-e container=docker 
-v /sys/fs/cgroup:/sys/fs/cgroup:rw 
--cgroupns=host ndy2/docker-server:m1 /usr/sbin/init
```
