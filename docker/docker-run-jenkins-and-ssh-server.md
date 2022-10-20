
### jenkins 
```
docker run -d  \
-p 8080:8080 -p 50000:50000  \
--name jenkins-server \
--restart=on-failure \
-v jenkins_home:/var/jenkins_home/jenkins/jenkins:lts-jdk11
```



### MacOS apple silicon chip, m1) SSH 서버 (with 도커) 실행 명령어
```
docker run \
--privileged \
--name docker-server -itd \
-p 10022:22 -p 8082:8080 \
-e container=docker \
-v /sys/fs/cgroup:/sys/fs/cgroup:rw \
--cgroupns=host edowon0623/docker-server:m1 /usr/sbin/init
```

```
ssh root@localhost -p 10022
P@ssw0rd
```

```
systemctl status docker
systemctl start docker
```

## MacOS silicon chip, m1)) Ansible 컨테이너 실행 명령어

```
docker run --rm \
--privileged -itd \
--name ansible-server \
-p 20022:22 -p 8081:8080 \
-e container=docker \
-v /sys/fs/cgroup:/sys/fs/cgroup:rw \
--cgroupns=host \
edowon0623/ansible-server:m1 /usr/sbin/init
```

```
ssh root@localhost -p 20022 (or docker exec -it ansible-server bash)
P@ssw0rd
```

```
ansible --version
vi /etc/ansible/hosts
```

```
yum install -y ncurses
-> clear 가능
```

```
ssh-keygen
ssh-copy-id root@[접속할 서버 IP]
```

