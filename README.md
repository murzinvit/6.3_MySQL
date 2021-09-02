### 6.3.-MySQL </br>
-------------------------------------------------------------------------------------
### Задача 1:
Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume: </br>
1) Загрузка контейнера: `docker pull mysql:latest` </br>
2) Запуск контейнера: `docker run -d -e MYSQL_ROOT_PASSWORD=pass -v ~/mysql-data:/var/lib/mysql --name mysrv mysql` </br>
3) Вход в контейнер: `docker exec -it mysrv /bin/bash` </br>

Изучите [бэкап БД](https://github.com/murzinvit/6.3_MySQL/blob/da7841c0c982eb7c4cddc2f5212a2801cd888445/test_dump.sql) и восстановитесь из него: </br>
1) Скопировать файл дампа в папку контейнера: `cp test_dump.sql ~/mysql-data` </br>
2) Создать базу test_db в контейнере: `mysql -u root -p`, `create database test_db character set utf8;` <\br>
3) Восстановление из test_dump.sql в test_db: `cd /var/lib/mysql`,  `mysql -u root p test_db < test_dump.sql` </br>

Перейдите в управляющую консоль mysql внутри контейнера: </br>
1) `docker exec -it mysrv /bin/bash` </br>
2) `mysql -u root -p` </br>

Используя команду \h получите список управляющих команд: </br>
Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД. </br>
#### Команда для получения статуса БД: `status;` </br>
![screen](https://github.com/murzinvit/screen/blob/abc2a3f6db5769308e626e899ca6fe028c36a206/mysql_status_result.jpg)</br>

Подключитесь к восстановленной БД и получите список таблиц из этой БД: </br>
1) `mysql -u root -p` </br>
2) `use test_db;` </br>
3) `show tables;` </br>

Приведите в ответе количество записей с price > 300: </br>
1) 1 запись с price > 300, запрос: `select price from orders where price > 300;` </br>
[screen](https://github.com/murzinvit/screen/blob/eeafd190f797ccf2e2680ff75c1bf13904d9026d/Mysql_select_result.jpg)

### Задача 2: </br>
Создайте пользователя test в БД c паролем test-pass, используя: </br> 
Документация по запросам [SQL](https://dev.mysql.com/doc/refman/8.0/en/alter-user.html) </br>
Также запросы хорошо ищуться по названиям полей из таблицы - `use mysql;` , `describe users;` </br>
1) Создать: `CREATE USER 'test'@'localhost'IDENTIFIED WITH mysql_native_password BY 'test-pass';` </br>
2) Срок действия пароля: `ALTER USER 'test'@'localhost' PASSWORD EXPIRE INTERVAL 180 DAY;`</br>
3) Попыток авторизации - 3: `ALTER USER 'test'@'localhost' FAILED_LOGIN_ATTEMPTS 3 PASSWORD_LOCK_TIME 2;`</br>
4) Максимальное кол-во запросов в час - 100: `ALTER USER 'test'@'localhost' WITH MAX_QUERIES_PER_HOUR 100;`[док](https://dev.mysql.com/doc/refman/8.0/en/alter-user.html#alter-user-resource-limits)</br>
5) Дать права на SELECT: `GRANT SELECT ON test_db.* TO 'test'@'localhost';` </br>
6) Просмотр прав пользователя: `SHOW GRANTS FOR 'test'@'localhost';` </br>

