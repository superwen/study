常用的compose文件的写法：
```
version: '2.1'

networks:
  huannet:     
  
services:
  mysql:
    image: mysql:5.7
    container_name: mysql01
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: "portalhuan2009"
    ports:
      - "3306:3306"  
    volumes:
      - ~/docker/data/mysql:/var/lib/mysql
  cockroach01:
    image: cockroachdb/cockroach:v2.1.6
    container_name: cockroach01
    restart: always   
    networks:
      - huannet
    command: start --insecure
    ports:
      - "26257:26257" 
      - "8081:8080"
    volumes:
      - ~/docker/data/cockroach-data01:/cockroach/cockroach-data 
  cockroach02:
    image: cockroachdb/cockroach:v2.1.6
    container_name: cockroach02
    restart: always   
    networks:
      - huannet
    command: start --insecure --join=cockroach01
    ports:
      - "26258:26257" 
      - "8082:8080"
    volumes:
      - ~/docker/data/cockroach-data02:/cockroach/cockroach-data 
  cockroach03:
    image: cockroachdb/cockroach:v2.1.6
    container_name: cockroach03
    restart: always   
    networks:
      - huannet
    command: start --insecure --join=cockroach01
    ports:
      - "26259:26257" 
      - "8083:8080"
    volumes:
      - ~/docker/data/cockroach-data03:/cockroach/cockroach-data        
```      
