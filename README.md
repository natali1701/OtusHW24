# **Mysql.**

## **Homework**

Развернуть дамп и настроить репликацию.

В материалах приложены ссылки на вагрант для репликации и дамп базы bet.dmp.

Базу развернуть на мастере и настроить чтобы реплицировались таблицы

| bookmaker |

| competition |

| market |

| odds |

| outcome

* Настроить GTID репликацию

Варианты которые принимаются к сдаче
- рабочий вагрантафайл
- скрины или логи SHOW TABLES

* конфиги

* пример в логе изменения строки и появления строки на реплике


## **Разворачиваем базу на мастере с помощью provisioning/playbook.yml**

Вносим изменения в конфигурационный файл:
- включаем GTID режим репликации, 
- определяем уникальный server_id,
- указываем таблицы  реплицирования на slave.

replicate-do-db=**bet**

replicate-do-table=bet.**bookmaker**

replicate-do-table=bet.**competition**

replicate-do-table=bet.**market**

replicate-do-table=bet.**odds**

replicate-do-table=bet.**outcome**

## **Проверка**

**1. MASTER**

Убеждаемся что GTID включен:

mysql> SHOW VARIABLES LIKE 'gtid_mode';


| Variable_name | Value |
| ------------- | :---- |
| gtid_mode     | ON    |


1 row in set (0.01 sec)


Выведем список баз:

mysql> SHOW DATABASES;


| Database           |
| ------------------ |
| bet                |
| information_schema |
| mysql              |
| performance_schema |
| sys                |


5 rows in set (0.01 sec)

Проверим только базу bet:

mysql> USE bet;

Reading table information for completion of table and column names

You can turn off this feature to get a quicker startup with -A

Database changed

**mysql> SHOW TABLES;**


| Tables_in_bet    |
| ---------------- |
| bookmaker        |
| competition      |
| events_on_demand |
| market           |
| odds             |
| outcome          |
| v_same_event     |


7 rows in set (0.01 sec)


**mysql> SELECT * FROM bookmaker;**


| id | bookmaker_name |
| -- | -------------- |
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |


5 rows in set (0.00 sec)



**2.SLAVE**

После поднятия vagrant, проверим корректность выполнения задач, что были поставлены перед ansible:

[vagrant@slave ~]$  cat /var/log/mysqld.log

2019-04-26T15:08:14.754205Z 10 [System] [MY-010562] [Repl] **Slave** I/O thread for channel '': **connected to master 'repl@master:3306',replication started** in log 'FIRST' at position 4

**Репликация успешно выполнена.**

[root@slave ~]# mysql -u root -p

Enter password: 

Welcome to the MySQL monitor.  Commands end with ; or \g.

Your MySQL connection id is 20

Server version: 8.0.16 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

mysql> SHOW VARIABLES LIKE 'gtid_mode';


| Variable_name | Value |
| ------------- | ----- |
| gtid_mode     | ON    |

1 row in set (0.02 sec)

Выведем список баз:

mysql> SHOW DATABASES;


| Database           |
| ------------------ |
| bet                |
| information_schema |
| mysql              |
| performance_schema |
| sys                |


4 rows in set (0.00 sec)

mysql> USE bet;

Reading table information for completion of table and column names

You can turn off this feature to get a quicker startup with -A

Database changed

**mysql> SHOW TABLES;**


| Tables_in_bet |
| ------------- |
| bookmaker     |
| competition   |
| market        |
| odds          |
| outcome       |

5 rows in set (0.00 sec)

**mysql> SELECT * FROM bookmaker;**


| id | bookmaker_name |
| -- | -------------- |
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |


5 rows in set (0.00 sec)

