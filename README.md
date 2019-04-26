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

**Разверачиваем базу на мастере с помощью provisioning/playbook.yml**

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

