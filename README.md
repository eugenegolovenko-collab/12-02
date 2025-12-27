# Домашнее задание к занятию "`Работа с данными (DDL/DML)`" - `Евгений Головенко`

---

### Задание 1

1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp. 

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)

1.4. Дайте все права для пользователя sys_temp. 

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)

1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*

### Решение 1

1.1. Поднят чистый инстанс MySQL версии 8.0. Был использован контейнер Docker. На локальном порту 3306 уже сидит другой mysql, поэтому использую свободный порт 3307.

```
docker pull mysql:8.0
```

```
docker run --name mysql8 \
  -e MYSQL_ROOT_PASSWORD=rootpass \
  -p 3307:3306 \
  -d mysql:8.0
```

Проверка статуса контейнера:

```
docker ps
```

ну или так:

<img width="1381" height="534" alt="Capture1" src="https://github.com/user-attachments/assets/eb0401e1-84b7-47b6-bb7b-45165b16d717" />


1.2. Создана учётная запись sys_temp.

Вход в контейнер:

```
docker exec -it mysql8 mysql -uroot -prootpass
```

Создание пользователя:

```
CREATE USER 'sys_temp'@'%' IDENTIFIED BY 'temp_pass';
```
*Использован `%`, а не `localhost` потому что Docker это не localhost внутри контейнера. `%` избавит потом от проблем с подключением из IDE.*

Проверка:

```
SELECT user, host FROM mysql.user;
```

1.3. Выполнен запрос на получение списка пользователей в базе данных. (скриншот)

<img width="550" height="244" alt="1_users" src="https://github.com/user-attachments/assets/cd9b1512-ea62-4f79-861d-3928d8d6c733" />

1.4. Даны все права для пользователя sys_temp.

```
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

а именно:

- выданы абсолютно все права (ALL PRIVILEGES);

- на все базы и таблицы (*.*);

- разрешено передавать права дальше (WITH GRANT OPTION).

1.5. Выполнен запрос на получение списка прав для пользователя sys_temp. (скриншот)

```
SHOW GRANTS FOR 'sys_temp'@'%';
```

<img width="1128" height="483" alt="2_PRIVILEGES" src="https://github.com/user-attachments/assets/1111e6ed-1660-4742-9061-41ee107d76f4" />

1.6. Переподключение к базе данных от имени sys_temp.

```
ALTER USER 'sys_temp'@'%'
IDENTIFIED WITH mysql_native_temp_pass
BY 'temp_pass';
```

Выход из MySQL и вход как sys_temp:

```
docker exec -it mysql8 mysql -usys_temp -ptemp_pass
```

<img width="697" height="430" alt="3_Change_user" src="https://github.com/user-attachments/assets/e9034250-2662-4dce-9e21-78603cc0dbfe" />

1.6. - 1.7 По [ссылке](https://downloads.mysql.com/docs/sakila-db.zip) скачен дамп базы данных и восстановлен дамп в базу данных.

<img width="860" height="102" alt="Capture2" src="https://github.com/user-attachments/assets/77b5745b-9f1a-4409-832b-b71032467ab1" />

```
docker exec -it mysql8 mysql -usys_temp -ptemp_pass
```

```
SOURCE /sakila-schema.sql;
SOURCE /sakila-data.sql;
```

<img width="496" height="682" alt="4_Show_tables" src="https://github.com/user-attachments/assets/3f4b7a1f-60f0-496e-ac47-9776d6e0c7ce" />

<img width="1487" height="1486" alt="ER_sakila" src="https://github.com/user-attachments/assets/fbf2a6f6-6f12-4c17-8ab0-59eafad60218" />

Простыня SQL-запросов (именно запросов, без Power shell команд):

```
-- Создание пользователя
CREATE USER 'sys_temp'@'%' IDENTIFIED BY 'temp_pass';

-- Список пользователей
SELECT user, host FROM mysql.user;

-- Назначение прав
GRANT ALL PRIVILEGES ON *.* TO 'sys_temp'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

-- Проверка прав
SHOW GRANTS FOR 'sys_temp'@'%';

-- Смена пользователя
ALTER USER 'sys_temp'@'%'
IDENTIFIED WITH mysql_native_password
BY 'temp_pass';

-- Наличие баз
SHOW DATABASES;

-- Действия с Sakila
USE sakila;
SHOW TABLES;

```
---

### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```

### Решение 2

<img width="233" height="423" alt="image" src="https://github.com/user-attachments/assets/b6c2a7b9-6013-4fab-8166-cc50168adbff" />

---
