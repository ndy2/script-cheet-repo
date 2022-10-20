### ansible basic

전체 모듈 핑 테스트

```
ansible all -m ping
```
![image](https://user-images.githubusercontent.com/67302707/196904422-266dfca4-6699-4140-a77d-b491417b3a02.png)


전체 모듈 메모리 확인

```
ansible all -m shell -a "free -h"
```


파일 전송

```
ansible all -m copy -a "src=./test.txt dest=/tmp"
```

yum install
```
ansible devops -m yum -a "name=httpd state=present"
```
