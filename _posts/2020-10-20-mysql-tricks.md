# mysql server run command
`docker run -p 3306:3306 --name mysql57 -v /data/mysql57:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=$PASSWORD -d mysql:5.7`

# mysql client
## local client installation
`apt install mysql-client`

## run
Or use mysql client in the docker.
```
docker exec -it mysql57 /bin/bash
mysql -h 127.0.0.1 -uroot -p
```

# Configuration
## connections
```
SHOW VARIABLES LIKE "max_connections";
SHOW VARIABLES LIKE "max_used_connections";
set global max_connections = 1000;

```

## timezone
```
show variables like '%time_zone%';
set global time_zone = '+8:00';
flush privileges;
```
