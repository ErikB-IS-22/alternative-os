# Лабораторная работа №1: базовая настройка PostgreSQL на Debian

Цель: Настроить окружение, установить PostgreSQL в Debian, освоить базовые приёмы администрирования системы и СУБД.

## 1. Подготовка среды

После того как Debian был установлен на виртуальную машину была обновлена система с помощью команд apt-get update и apt-get upgrade. apt-get update обновляет локальный кэш списка доступных пакетов и их версий, а apt-get upgrade устанавливает новые версии уже установленных пакетов на основе текущего кэша метаданных. При обновлении системы всегда сначала выполняется apt-get update для актуализации списка пакетов, затем apt-get upgrade для установки обновлений.

<img width="974" height="496" alt="image" src="https://github.com/user-attachments/assets/8b04668b-6c78-4c7b-b258-6451dd240fd6" />

## 2. Установка PostgreSQL

Был установлен PostgreSQL через репозиторий с помощью пакетного менеджера apt командой sudo apt install postgresql postgresql-contrib. Пакет postgresql включает основной сервер и конфигурационные файлы, а пакет postgresql-contrib предоставляет дополнительные расширения и утилиты.

<img width="974" height="580" alt="image-1" src="https://github.com/user-attachments/assets/b7939906-d924-4c1a-8557-1921b44976c7" />

Также был установлен postgresql-client. Пакет postgresql-client содержит клиентские утилиты для взаимодействия с сервером PostgreSQL, запущенным локально или на удалённой машине.

<img width="974" height="323" alt="image-2" src="https://github.com/user-attachments/assets/d2aef4c0-84d5-42bb-8e8a-92e140feea13" />

## 3. Создание служебной учётной записи

Проверка что учётная запись postgres была успешно создана с помощью команды для доступа к командной строке Postgres.

<img width="559" height="234" alt="image-4" src="https://github.com/user-attachments/assets/c35a1b8a-487e-4a52-9eb2-8f4241ccedce" />

Учётная запись postgres в Debian — это системный пользователь, создаваемый пакетом postgresql-common при установке PostgreSQL.

Основные права и возможности учетной записи postgres:

- Полный контроль (Superuser): Может обходить любые проверки прав доступа, включая GRANT, REVOKE и ALTER DEFAULT PRIVILEGES.
- Управление объектами: Владелец или создатель большинства объектов в системных схемах по умолчанию.
- Управление базами данных: Может создавать, изменять и удалять любые базы данных (DROP/CREATE DATABASE).
- Управление ролями: Создание, изменение, удаление пользователей и групп (CREATE/DROP ROLE).
Назначение:
- Администрирование: Основная учетная запись для настройки сервера, резервного копирования и управления безопасностью.
- Первичная настройка: Используется для создания других ролей и баз данных сразу после установки.

Права и назначение этой учётной записи определены архитектурой безопасности СУБД.

## 4. Первичная настройка конфигурационных файлов

Далее с помощью nano были открыты основные файлы конфигурации postgresql.conf и pg_hba.conf которые находятся по пути /ect/postgresql/15/main/. В файл pg_hba.conf был отключен метод аутентификации путём замены peer на trust. Метод peer выполняет аутентификацию через имя системного пользователя ОС. Подключение к PostgreSQL от имени роли postgres разрешается только тогда, когда системный пользователь также называется postgres. Метод trust отключает аутентификацию полностью. Любое локальное подключение через Unix-сокет будет разрешено без проверки учётных данных.

<img width="956" height="33" alt="image-8" src="https://github.com/user-attachments/assets/894496d8-87e7-44a4-bc4e-eae5f6101495" />

<img width="902" height="30" alt="image-5" src="https://github.com/user-attachments/assets/50e5f059-d2a8-468e-a99b-8d3917882c95" />

<img width="974" height="75" alt="image-6" src="https://github.com/user-attachments/assets/36a8a3cc-b421-4ade-a173-18ea9aba8fc4" />

<img width="974" height="65" alt="image-7" src="https://github.com/user-attachments/assets/58d07bfb-a227-44e8-a5b6-982277ecebb3" />


## 5. Управление сервисом

Был проверен статус и был включен автозапуск PostgreSQL с помощью systemctl. systemctl - это утилита командной строки для управления системой и службами в Linux-дистрибутивах, использующих систему инициализации systemd. Видно, что служба активна.

<img width="974" height="426" alt="image-9" src="https://github.com/user-attachments/assets/de6741e1-5e10-429d-88f0-a3c93200c3e7" />

## 6. Создание тестовой базы данных

В PostgreSQL через суперпользователя был создан отдельный пользователь bainazarovei и база данных dbbainazarovei. Был задан пароль пользователю bainazarovei.

<img width="872" height="105" alt="image-3" src="https://github.com/user-attachments/assets/156ea406-5011-461d-9130-263df7d2a283" />

<img width="728" height="213" alt="image-10" src="https://github.com/user-attachments/assets/ffebfcb2-70b4-40a5-91b4-9544f47f72ec" />

С помощью команды \du было проверено наличие пользователя

<img width="211" height="30" alt="image-11" src="https://github.com/user-attachments/assets/818d6d84-c8a5-4f4a-b4bb-ffcaaa4c49f3" />

<img width="974" height="120" alt="image-12" src="https://github.com/user-attachments/assets/b1173cb6-e645-46b0-86bd-3f9e9cdb3eed" />

## 7. Знакомство со схемами

Создание новой схемы с помощью CREATE SCHEMA и установка прав пользователю на использование схемы с помощью GRANT.

<img width="874" height="67" alt="image-13" src="https://github.com/user-attachments/assets/d496adba-c41f-4ef4-89d8-564f89262a53" />

Проверка текущего пути.

<img width="406" height="167" alt="image-14" src="https://github.com/user-attachments/assets/8b86e3d6-880e-4493-bc65-54e9fbea6a51" />

Смена search_path для сессии чтобы обращаться к таблицам без указания имени схемы.



Смена search_path для пользователя.

<img width="974" height="66" alt="image-16" src="https://github.com/user-attachments/assets/22459d03-35eb-4c24-804d-ec5261429c96" />

В PostgreSQL база данных и схема — это два разных уровня изоляции объектов. База данных — это независимый контейнер на уровне кластера СУБД. Каждая база данных имеет собственное пространство имён, системные каталоги и набор объектов. Схема — это логический контейнер внутри одной базы данных. Она группирует объекты: таблицы, представления, функции, типы. Одна база данных может содержать множество схем.

## 8. Использование утилиты psql для базовых операций

Создание таблицы в схеме public с помощью запроса CREATE TABLE.

<img width="659" height="161" alt="image-17" src="https://github.com/user-attachments/assets/b5e288eb-de91-4782-ab30-8ccc2cce0473" />

SQL-запросы SELECT, INSERT, UPDATE, DELETE.

<img width="628" height="627" alt="image-18" src="https://github.com/user-attachments/assets/f4ac1272-8ff7-4fb3-afeb-d9e864e7e375" />

<img width="525" height="431" alt="image-19" src="https://github.com/user-attachments/assets/f9fefd2b-3a02-4a02-adcc-c95bec0940c3" />

Создание таблицы в схеме test_schema с помощью запроса CREATE TABLE с привязкой к конкретиной схеме.

<img width="706" height="166" alt="image-20" src="https://github.com/user-attachments/assets/2a1e2251-2fc0-4e85-8d3f-0a2d196bf893" />

SQL-запросы SELECT, INSERT, UPDATE, DELETE.



<img width="697" height="433" alt="image-22" src="https://github.com/user-attachments/assets/0b264653-e7d2-4bd0-8419-5eef3866238e" />

## 9. Настройка локальных и сетевых подключений

Определение внутреннего IP сервера для настройки доступа к базе данных по локальной сети с помощью ip a.

<img width="974" height="293" alt="image-23" src="https://github.com/user-attachments/assets/5f5e7e5a-805f-4c95-82c3-54103fb7096e" />

Смена параметра listen_addresses на конкретный внутренний IP сервера, который определяет, на каких сетевых интерфейсах сервер будет принимать подключения.

<img width="974" height="92" alt="image-24" src="https://github.com/user-attachments/assets/aaf6e4b5-39b1-4c0a-a698-f678a86a53dc" />

Смена адреса в правилах аутентификации, чтобы разрешались подключения с локальной сети. Каждая строка определяет правило доступа. Формат: тип-база-пользователь-адрес-метод.

<img width="704" height="46" alt="image-26" src="https://github.com/user-attachments/assets/685d1855-a57e-4d4e-97fa-8b9e02971a92" />

После изменения обоих файлов был перезапущен PostgreSQL. Был установлен ufw и было задано разрешение входящих подключений на порт.

<img width="974" height="384" alt="image-25" src="https://github.com/user-attachments/assets/e60f5db3-faf2-4eed-8247-0234f0aa2bad" />

<img width="956" height="64" alt="image-27" src="https://github.com/user-attachments/assets/02672a1c-4436-4214-878d-6c2751f24136" />

Был изменён тип подключения на сетевой мост в настройках виртуальной машины.

<img width="974" height="436" alt="image-28" src="https://github.com/user-attachments/assets/d431379a-c9cc-4d24-8b02-84e256c666ae" />

В настройках подключения DBeaver был указан внутренний IP сервера, база данных dbbainazarovei, пользователь bainazaroei и пароль.

<img width="974" height="846" alt="image-29" src="https://github.com/user-attachments/assets/f4b5fe7b-5026-4b56-b293-d6347841b330" />

## 10. Журналирование (logging)

Для изменения настроек журналирования был открыт файл postgresql.conf и были заданы определённые параметры:

- `logging_collector` включает сборщик логов, который перенаправляет stderr в файлы. Значение `on` активирует запись в файлы вместо syslog.
- `log_directory` указывает директорию для логов относительно data directory PostgreSQL.
- `log_filename` определяет формат имени файла. Указанный шаблон создаст файлы с датой и временем в имени, что удобно для ротации.
- `log_statement` со значением `all` записывает все SQL-запросы. Другие варианты: `none`, `ddl` (только DDL-команды), `mod` (DDL плюс INSERT/UPDATE/DELETE). Для продакшена `all` создаст большой объем логов, но для отладки это полезно.
- `log_connections` и `log_disconnections` записывают события подключения и отключения клиентов.
- `log_duration` добавляет время выполнения каждого запроса.
- `log_line_prefix` форматирует префикс каждой строки лога. Используемые подстановки: `%t` — timestamp, `%p` — process ID, `%l` — номер строки лога, `%u` — имя пользователя, `%d` — база данных, `%a` — имя приложения, `%h` — хост клиента.
- `log_min_duration_statement` со значением `0` логирует все запросы независимо от времени выполнения.

<img width="974" height="310" alt="image-30" src="https://github.com/user-attachments/assets/5b1d569d-5bfb-4bc7-b458-7ecc24dc0a79" />
<img width="974" height="235" alt="image-31" src="https://github.com/user-attachments/assets/743d679d-d9b7-4936-a265-5cc32e779741" />
<img width="974" height="117" alt="image-32" src="https://github.com/user-attachments/assets/b2601f26-3f62-4de1-a3a0-5246e8e790a6" />

Были просмотрены логи по пути /var/log/postgresql/postgresql-15-main.log.

<img width="974" height="182" alt="image-33" src="https://github.com/user-attachments/assets/4e9c015d-d362-4cb3-a37e-1da415ae4c61" />

## 11. Назначение ролей и прав

Был создан пользователь с ограниченными привелегиями limited_user (только с правом входа и пароля) с помощью запроса WITH LOGIN PASSWORD.

<img width="925" height="94" alt="image-34" src="https://github.com/user-attachments/assets/3a3a24c9-0597-47ad-97d3-80fd727cd34a" />

Роль не имеет права читать данные из таблицы, хотя может подключаться к базе.

<img width="974" height="226" alt="image-35" src="https://github.com/user-attachments/assets/4167956f-0ee2-4276-96f2-6dae06076931" />

Пользователю limited_user были выданы права на использование SELECT с помощью GRANT. Команда GRANT используется для предоставления привилегий на объекты базы данных.



От имени limited_user были проверены права. Видно, что пользователь может использовать запрос SELECT, но не может использовать другие запросы.



Далее была создана группа ролей readers, которой были выданы права на использование SELECT. Для проверки наследования ролей был создан пользователь reader1 с ключевым словом INHERIT, которая означает, что роль автоматически получает все привилегии ролей, членом которых она является. Это поведение по умолчанию.



Видно, что пользователь может использовать запрос SELECT, но не может использовать другие запросы.


