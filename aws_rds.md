# Облачные СУБД

Варианты развертывания СУБД:

* Установить СУБД на оборудование on-Premise
* Установить СУБД в контейнер на оборудование on-Premise
* Установить СУБД в инстансе
* Установить СУБД в контейнере на инстансе
* Использовать СУБД в контейнере в облачном сервисе
* **Использовать облачный сервис**

Преимущества облачного сервиса:

* автоматическое масштабирование
* легкость резервного копирования
* производительность при масштабировании серверов приложения
* не нужно поддерживать ОС и ПО СУБД

Облачные СУБД:

* Реляционные

  - Amazon Aurora Serverless
  - Amazon Aurora (умеет работать в режиме MySQL и PostgreSQL)
  - MySQL
  - MariaDB
  - Oracle
  - Microsoft SQL Server
  - PostgreSQL

* Нереляционные (NoSQL)

  - document (AWS DocumentDB (MongoDB))
  - key-value (Amazon MemoryDB (Redis), Amazon DynamoDB)
  - wide-column store (Amazon Keyspaces (Cassandra), Amazon Redshift)
  - graph (Amazon Neptune)
  - time (Amazon Timestream)

Практическая часть

1. Аутентифицироваться в https://console.aws.amazon.com/
2. Открыть сервис RDS

  * Создать базу данных типа PostgreSQL
  * Имя инстанса: <группа>-<фамилия>
  * Имя пользователя: strapi
  * DB instance class: db.t3.micro
  * Storage type: General Purpose
  * Multi-AZ: no
  * Security Group: existing, default
  * Отключить Monitoring

3. Создать

  * виртуальную машину в EC2 типа t2.micro, Ubuntu 20.04, задать тег Name
  * установить туда Docker и Docker-Compose
  * скачать образ: `docker pull strapi/strapi`
  * написать docker-compose.yaml

```yaml
version: '3'
services:
  strapi:
    image: strapi/strapi
    ports: [80:1337]
    volumes: [./app:/srv/app]
    environment:
      DATABASE_CLIENT: postgres
      DATABASE_HOST: <DNS-имя вашей СУБД в RDS>
      DATABASE_PORT: 5432
      DATABASE_NAME: strapi
      DATABASE_USERNAME: strapi
      DATABASE_PASSWORD: 12345678
      DATABASE_SSL: 'false'
```

  * создать БД:
```bash
sudo apt-get install -y postgresql-client-12
psql -h <СУБД> -U strapi postgres
```
```
CREATE DATABASE strapi;
\q
```

  * запустить приложение: `docker-compose up -d`
  * проверить работу в браузере

4. Подключиться к `strapi` и настроить 2 таблицы с произвольными полями
