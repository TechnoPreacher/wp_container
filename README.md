Разворачивание проекта (docker)
=
---
####Общие сведения:

Представленная мультиконтейнерная сборка содержит три контейнера:
* apache + php 7.4 (без xdebug)
* mariadb, с пустой базой sgds_dev внутри
* phpmyadmin

Взаимодействие организовано через networks

Конкретные команды/линки/каталоги указаны для примера

---

А. Установка Docker
-
* необходимо установить Docker Desktop https://www.docker.com/products/docker-desktop/
* Docker Desktop всегда должен быть запущен

Б. Локальное размещение сборки
-
* всё содержимое этого каталога (docker-compose.yml, папка docker, папка app) разместить локально;
* в папке docker/etc лежат файлы настроек сервисов в контейнерах
* в папке docker/build лежат файлы
* в папке app/wordpress будет развёрнут рабочий проект

В. Клонирование проекта
-
* перейти в каталог app/wordpress (он должен быть полностью пустым, без скрытых файлов)
```
cd /users/ns/docbuild/app/wordpress
```
* используя git clone архив проекта (взяв ssh-конструкцию из github или bitbucket)
```
git clone git@bitbucket.org:Alexsolovei/ukad-sgds.git .
```
* в зависимости от проекта, нужно установить зависимости:
```
composer install
```
* в зависимости от проекта, нужно положить в app/wordpress и заполнить .env файл
```
DB_NAME=sgds_dev
DB_USER=root
DB_PASSWORD=qwerty
DB_HOST=mariadb
DB_PREFIX=wp_
WP_ENV=development
WP_HOME=http://sgds.local
WP_SITEURL=${WP_HOME}/wp
```
***тут имя БД, логин/пароль, хост - те же, что прописаны в docker-compose.yml

---

### Важное дополнение
в зависимости от состояния хранилища, бывает полезно переключиться на другую его ветку
```
git checkout develop
```
---

Г. Запуск docker-сборки
-

* в терминале перейти в каталог с docker-compose.yml и запустить docker-compose (клиент Docker Desktop должен быть открыт!)
```
cd ..
cd ..
docker-compose up -d
```
в случае успеха будут сообщения о подключенных контейнерах
```
Creating sgds_mariadb ... done
Creating sgds_apache     ... done
Creating sgds_phpmyadmin ... done
```
а в Docker Desktop появится мультисборка с тремя контейнерами внутри


---

### Замечание
* при переходе по http://localhost:8001 откроется phpmyadmin (root qwerty)
* при переходе http://localhost откроется проект

---

Д. Доступ
-

* для того, что вместо localhost использовать свой домен следует дополнить файл hosts (sudo nano /private/etc/hosts) содержимым

```
127.0.0.1 sgds.local
```
* теперь к своему проекту можно попасть по ссылке http://sgds.local
* phpmyadmin откроется по ссылке http://sgds.local:8001


E.  Дальнейшая работа
-

* если импорта БД не было, следует заполнить мастер настройки WP-проекта, перейдя по http://sgds.local
* активировать корректную тему;
* установить/активировать плагины (ACF!);
* выполнить дальнейшую работу по проекту, например стилизацию (работая в терминале из каталога с темой!)

```
cd /users/ns/docbuild/app/wordpress/wp-content/themes/sgds-theme
npm install
```

Ж. Полезные дополнения
-

* В терминале можно посмотреть содержимое СУБД

```
docker exec -it sgds_mariadb bash
mysql -uroot -pqwerty
show databases;
exit
exit
```
и убедится что создана нужная БД
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sgds_dev           |
+--------------------+
```


* при наличии .sql - файла проекта можно импортировать данные в контейнер
```
docker exec -i sgds_mariadb mariadb sgds_dev -uroot -pqwerty --default-character-set=utf8 < /path/to/sgds-backup-filename.sql
```

* отключить сборку можно, выполнив в терминале команду
```
docker-compose down   
```

Ж. По содержимому docker-compose.yml
-

### under construction
