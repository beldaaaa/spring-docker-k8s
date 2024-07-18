# Images
* `spring-gateway:0.0.1-SNAPSHOT`
* `spring-auth:0.0.1-SNAPSHOT`
* `spring-rest-mvc:0.0.1-SNAPSHOT`
* `spring-r2dbc:0.0.1-SNAPSHOT`
* `spring-reactive-mongo:0.0.1-SNAPSHOT`

Run Gateway
```shell
docker run --name gateway -d -p 8080:8080 spring-gateway:0.0.1-SNAPSHOT
```

Remove Container
```shell
docker rm gateway
```

Run Auth Server
```shell
docker run --name auth -d -p 9000:9000 spring-auth:0.0.1-SNAPSHOT
```

Run Rest MVC
```shell
docker run --name rest-mvc -d -p 8081:8081 spring-rest-mvc:0.0.1-SNAPSHOT
```

Show logs for Rest MVC
```shell
docker logs rest-mvc
```

Run Rest MVC
```shell
docker run --name rest-mvc -d -p 8081:8080 spring-rest-mvc:0.0.1-SNAPSHOT
```

Run Rest MVC on port 8081
```shell
docker run --name rest-mvc -d -p 8081:8081 -e SERVER_PORT=8081 spring-rest-mvc:0.0.1-SNAPSHOT
```

Run Rest MVC with profile localmysql
```shell
docker run --name rest-mvc -d -p 8081:8081 -e SPRING_PROFILES_ACTIVE=localmysql spring-rest-mvc:0.0.1-SNAPSHOT
```

Run Gateway with active profile docker
```shell
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker spring-gateway:0.0.1-SNAPSHOT

docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker --link auth:auth spring-gateway:0.0.1-SNAPSHOT
```

Run Auth Server with host name set to auth-server
```shell
docker run --name auth -h auth -d -p 9000:9000 spring-auth:0.0.1-SNAPSHOT
```

Run rest-mvc with jwt issuer host set and link
```shell
docker run --name rest-mvc -d -p 8081:8080 -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 --link auth:auth spring-rest-mvc:0.0.1-SNAPSHOT
```

Rerun gateway with link to auth-server and rest-mvc
```shell
docker stop gateway 
docker rm gateway
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker --link auth:auth --link rest-mvc:rest-mvc spring-gateway:0.0.1-SNAPSHOT
```

Run MySQL
```shell
docker run --name mysql -d -e MYSQL_USER=restadmin -e MYSQL_PASSWORD=password -e MYSQL_DATABASE=restdb -e MYSQL_ROOT_PASSWORD=password mysql:8
```

Run rest-mvc with link to mysql
```shell
docker stop rest-mvc
docker rm rest-mvc
docker run --name rest-mvc -d -p 8081:8080 -e SPRING_PROFILES_ACTIVE=localmysql \
 -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 -e SPRING_DATASOURCE_URL=jdbc:mysql://mysql:3306/restdb  \
 -e SERVER_PORT=8080 --link auth:auth --link mysql:mysql spring-rest-mvc:0.0.1-SNAPSHOT
```

Run r2dbc Container
```shell    
docker run --name r2dbc -d -p 8082:8080 -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 \
 --link auth:auth spring-r2dbc:0.0.1-SNAPSHOT
```

Rerun gateway with link to auth-server and rest-mvc and r2dbc
```shell
docker stop gateway 
docker rm gateway
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker --link auth:auth --link rest-mvc:rest-mvc \
--link r2dbc:r2dbc spring-gateway:0.0.1-SNAPSHOT
```

Run MongoDB
```shell
docker run -d --name mongo -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=example -p 27017:27017 mongo 
```

Run Reactive Mongo
```shell
docker run --name reactive-mongo -d  -e SPRING_SECURITY_OAUTH2_RESOURCESERVER_JWT_ISSUER_URI=http://auth-server:9000 \
 -e SFG_MONGOHOST=mongo -e SERVER_PORT=8080 --link auth:auth --link mongo:mongo spring-reactive-mongo:0.0.1-SNAPSHOT
```
Rerun gateway with link to auth-server and rest-mvc and r2dbc and reactive-mongo
```shell
docker stop gateway 
docker rm gateway
docker run --name gateway -d -p 8080:8080 -e SPRING_PROFILES_ACTIVE=docker --link auth:auth --link rest-mvc:rest-mvc \
--link r2dbc:r2dbc --link reactive-mongo:reactive-mongo spring-gateway:0.0.1-SNAPSHOT
```










