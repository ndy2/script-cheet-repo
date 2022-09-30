```
CREATE USER 'root'@'172.18.0.1' IDENTIFIED BY 'deukyun';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.18.0.1' WITH GRANT OPTION;
flush privileges;
```