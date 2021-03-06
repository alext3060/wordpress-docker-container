#  Wordpress website using Docker containers

Its just a documentation to create wordpress website and phpMyADMIN using docker containers. Here, Created a docker composer to provision three containeers ( for: wordpress files, database and phpMyADMIN)
### Prerequisites

Install docker service on the server. 

```
#sudo apt get update
#sudo apt install docker.io
#service docker restart
#docker --version
```

Install docomper composer on the server:

```
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Add the code below to a file called "docker-compose.yaml" and run the command:

```
$ docker-compose up -d
```
```

version: '3'
services:
 #Database
 db:
  image: mysql:5.7
  restart: always
  volumes:
   - db_data:/var/lib/mysql
  environment:
   MYSQL_ROOT_PASSWORD: test123
   MYSQL_DATABASE: wordpress_db
   MYSQL_USER: wordpress_user
   MYSQL_PASSWORD: test321
  networks:
   - wordpress_net
  #phpMyADMIN
 phpmyadmin:
  depends_on:
   - db
  image: phpmyadmin/phpmyadmin
  restart: always
  ports:
   - '8000:40'
  environment:
   PMA_HOST: db
   MYSQL_ROOT_PASSWORD: test123
  networks:
   - wordpress_net
 #wordpress_files
 wordpress:
  depends_on:
   - db
  image: wordpress:latest
  ports:
   - '8001:80'
  restart: always
  volumes:
   - wordpress_data:/var/www/html
  environment:
   WORDPRESS_DB_HOST: db:3306
   WORDPRESS_DB_USER: wordpress_user
   WORDPRESS_DB_PASSWORD: test321
  networks:
   - wordpress_net
networks:
 wordpress_net:
volumes:
 db_data:
 wordpress_data:
```

