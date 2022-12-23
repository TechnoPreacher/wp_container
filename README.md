DOCKER MULTICONTAINER APP
=


---
#### features: 

* apache + php + xdebug3 (PHP's version set by ${PHP_VERSION})
* mariadb, (create empty ${PROJECT_NAME}_db database)
* phpmyadmin
* .env variables
---

Docker install
-

* Get Docker Desktop https://www.docker.com/products/docker-desktop/
* Docker Desktop should be always run


Deploy
-

* create storage folder and put all content from this pack to it
* infill .env with property settings
* create wordpress in app folder with right rights (folder wordpres must be empty)
* use "wordpress 5min instalation" in app/wordpres folder or clone project repository 
* don't forget to fill wp_config.php with settings equal to root .env

```
DB_NAME=...
DB_USER=...
DB_PASSWORD=...
DB_HOST=mariadb
DB_PREFIX=wp_
WP_ENV=development
WP_HOME=http://...
WP_SITEURL=${WP_HOME}/wp
```

Containers bring to life
-

* run docker-compose from cmd in storage folder
```
docker-compose up -d
```
* check Docker Desktop for Container's list

* shutdown:
```
docker-compose down   
```

Links
-

http://localhost:8000 - default link for wordpress project  
http://localhost:8001 - default link for phpmyadmin  

Improvements
-

put some changes to local hosts file for domain-style access
```
127.0.0.1 VAR.local
```

make import for .sql 
```
docker exec -i new_mariadb mariadb new_dev -uroot -pqwerty --default-character-set=utf8 < /path/to/backup-filename.sql
```

Debug
-
make proper maping when first use debug in PHPSTORM
