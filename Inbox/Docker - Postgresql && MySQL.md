2025013123:46
___
Date: 31-01-2025 | 23:46
Tags: #docker #postgresql
mapofcontents: 
___
## Postgresql in docker

_run in terminal_
`$ docker run --name tt-postgres  -e POSTGRES_PASSWORD=1111 -p 5431:5432 -d postgres`

### Дополнительные настройки (опционально)

Если нужно создать базу данных и пользователя при запуске контейнера, можно добавить переменные окружения:

```bash
docker run --name tt-postgres \
  -e POSTGRES_PASSWORD=1111 \
  -e POSTGRES_DB=mydb \
  -e POSTGRES_USER=user \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 \
  -d postgres
```


- `--name tt-postgres`: Задаёт имя контейнера `tt-postgres`.
- `-e POSTGRES_PASSWORD=1111`: Устанавливает пароль для пользователя `postgres` (по умолчанию это суперпользователь). В данном случае пароль — `1111`.
- `-e POSTGRES_DB=mydb`: Создаёт базу данных с именем `mydb`.
- `-e POSTGRES_USER=user`: Создаёт пользователя `user`.
- `-e POSTGRES_PASSWORD=password`: Устанавливает пароль для пользователя `user`.
- `-p 5432:5432`: Пробрасывает порт 5432 контейнера на порт 5432 хоста. PostgreSQL по умолчанию использует порт 5432.
- `-d`: Запускает контейнер в фоновом режиме (detached).
- `postgres`: Указывает образ Docker для PostgreSQL.

---
### Подключение к PostgreSQL

После запуска контейнера можно подключиться к PostgreSQL с помощью команды:
`psql -h 127.0.0.1 -p 5432 -U user -d mydb`

Или через контейнер:
`docker exec -it tt-postgres psql -U user -d mydb`

---

### Остановка и удаление контейнера

- Остановить контейнер:
	`docker stop tt-postgres`
- Удалить контейнер:
	`docker rm tt-postgres`

---
## MySQL in docker

_run in terminal_
`docker run --name tt-mysql -e MYSQL_ROOT_PASSWORD=1111 -p 3306:3306 -d mysql`

- `--name tt-mysql`: Задаёт имя контейнера `tt-mysql`.
- `-e MYSQL_ROOT_PASSWORD=1111`: Устанавливает переменную окружения `MYSQL_ROOT_PASSWORD`, которая задаёт пароль для пользователя `root`. В данном случае пароль — `1111`.
- `-p 3306:3306`: Пробрасывает порт 3306 контейнера на порт 3306 хоста. MySQL по умолчанию использует порт 3306.
- `-d`: Запускает контейнер в фоновом режиме (detached).
- `mysql`: Указывает образ Docker для MySQL.

---
### Дополнительные настройки (опционально)

Если нужно создать базу данных и пользователя при запуске контейнера, можно добавить переменные окружения:

```bash
docker run --name tt-mysql \
  -e MYSQL_ROOT_PASSWORD=1111 \
  -e MYSQL_DATABASE=testDB \
  -e MYSQL_USER=milk \
  -e MYSQL_PASSWORD=1111 \
  -p 3306:3306 \
  -v /path/to/local/data:/var/lib/mysql \
  -d mysql:8.0 \
  --default-authentication-plugin=mysql_native_password
```

- `MYSQL_DATABASE=mydb`: Создаёт базу данных с именем `mydb`.
- `MYSQL_USER=user`: Создаёт пользователя `user`.
- `MYSQL_PASSWORD=password`: Устанавливает пароль для пользователя `user`.

---
### Подключение к MySQL

После запуска контейнера можно подключиться к MySQL с помощью команды:
`mysql -h 127.0.0.1 -P 3306 -u root -p`

Или через контейнер:
`docker exec -it tt-mysql mysql -u root -p`

--- 
### Остановка и удаление контейнера

- Остановить контейнер:
	`docker stop tt-mysql`
- Удалить контейнер:
	`docker rm tt-mysql`


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
