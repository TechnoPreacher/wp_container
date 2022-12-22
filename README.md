Разворачивание проекта (docker)
=

---
#### Общие сведения:

Представленная мультиконтейнерная сборка содержит три контейнера:
* apache + php 7.4 + xdebug3
* mariadb, с пустой базой ${PROJECT_NAME}_dev внутри
* phpmyadmin

* все настройки лежат в .env-файле в корне

Взаимодействие организовано через networks

Конкретные команды/линки/каталоги указаны для примера

---

А. Установка Docker
-
* необходимо установить Docker Desktop https://www.docker.com/products/docker-desktop/
* Docker Desktop всегда должен быть запущен

Б. Локальное размещение сборки
-
* всё содержимое этого каталога (docker-compose.yml, папка docker, папка app, файл .env ) разместить локально;
* в папке docker/etc лежат файлы настроек сервисов в контейнерах
* в папке docker/build лежат файлы сборки контейнеров
* в папке app/wordpress будет развёрнут рабочий проект

ЗАПОЛНИТЬ СОДЕРЖИМОЕ .env ФАЙЛА в корне
```
PROJECT_NAME=new
ROOT_PASSWORD=123
REG_USER=use
REG_PASSWORD=111
```
* Доступ к проекту в будущем можно получить из браузера по new.local или localhost



В. Клонирование проекта
-
* перейти в каталог app/wordpress (он должен быть полностью пустым, без скрытых файлов)
```
cd /users/ns/docbuild/app/wordpress
```
* используя git clone архив проекта (взяв ssh-конструкцию из github или bitbucket)
```
git clone {github or bitbucket link} .
```
* в зависимости от проекта, нужно установить зависимости:
```
composer install
```
* в зависимости от проекта, нужно либо положить в app/wordpress и заполнить .env файл правильными настройками (есть пример в app)
```
DB_NAME=...
DB_USER=root
DB_PASSWORD=...
DB_HOST=mariadb
DB_PREFIX=wp_
WP_ENV=development
WP_HOME=http://...
WP_SITEURL=${WP_HOME}/wp
```

***тут имя БД, логин/пароль, хост - те же, что прописаны в docker-compose.yml

* либо правильо заполнить wp-config.php в app/wordpress (если разворачиваем пустой проект; пример есть в app/wp-config.sample.php)

```
define( 'DB_NAME', 'new_db' );
define( 'DB_USER', 'use' );
define( 'DB_PASSWORD', '111' );
```


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
Creating new_mariadb ... done
Creating new_apache     ... done
Creating new_phpmyadmin ... done
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
127.0.0.1 new.local
```
* теперь к своему проекту можно попасть по ссылке http://new.local
* phpmyadmin откроется по ссылке http://new.local:8001


E.  Дальнейшая работа
-

* если импорта БД не было, следует заполнить мастер настройки WP-проекта, перейдя по http://new.local
* активировать корректную тему;
* установить/активировать плагины (ACF!);
* выполнить дальнейшую работу по проекту, например стилизацию (работая в терминале из каталога с темой!)

```
cd /users/ns/docbuild/app/wordpress/wp-content/themes/${YOUR_THEME}
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
| new_db             |
+--------------------+
```


* при наличии .sql - файла проекта можно импортировать данные в контейнер
```
docker exec -i new_mariadb mariadb new_dev -uroot -pqwerty --default-character-set=utf8 < /path/to/backup-filename.sql
```

* отключить сборку можно, выполнив в терминале команду
```
docker-compose down   
```

З. По отладке
-

* При первом запуске отладки в PHPSTORM правильно сделать мапинг (указать путь к файлу, где проставлен брекпоинт)


Е. По содержимому docker-compose.yml
-

### under construction
