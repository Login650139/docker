# Лабораторная работа Docker

## Цель


## Создание репозитория

```bash
git init
git branch -M main
git remote add origin https://github.com/Login650139/docker.git
```

Команды создают локальный репозиторий и подключают его к GitHub.

## Структура проекта

```bash
mkdir -p app db
```

Создаются каталоги для приложения и базы данных.

## Зависимости приложения

```bash
cat > app/requirements.txt
```

## Web-приложение

```bash
cat > app/app.py
```

Создано Flask-приложение, которое подключается к базе данных, читает записи из таблицы `tasks` и выводит их на страницу.

В браузере отображается:

```text
Список из Базы Данных
Пример 1
Пример 2
```

## Dockerfile

```bash
cat > Dockerfile
```

Создан `Dockerfile`, который собирает контейнер с Python-приложением.

Основные действия Dockerfile:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app/ .
CMD ["python", "app.py"]
```

Он устанавливает зависимости, копирует код приложения и запускает `app.py`.

## Файл базы данных

```bash
cat > db/init.sql
```

Создан файл инициализации MySQL.
Он создаёт базу `labdb`, таблицу `tasks` и добавляет начальные записи:

```sql
INSERT INTO tasks (name) VALUES
('Пример 1'),
('Пример 2');
```

## Docker Compose

```bash
cat > docker-compose.yml
```

Создан файл `docker-compose.yml`, который запускает два сервиса:

```text
app — web-приложение
db — база данных MySQL
```
Порт:

```yaml
ports:
  - "9888:5000"
```

Это значит, что приложение внутри контейнера работает на `5000`, а в браузере открывается через:

```text
http://localhost:9888
```

Для MySQL используется файл:

```yaml
./db/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
```

Он автоматически создаёт таблицу и начальные данные при запуске контейнера.

## Проверка Docker

```bash
sudo docker build -t lab-docker .
```

Команда собирает Docker-образ приложения.

```bash
sudo docker run -d --name lab_docker_single -p 9888:5000 lab-docker
```

Команда запускает контейнер с приложением.

```bash
sudo docker cp README.md lab_docker_single:/home/README.md
```

Команда копирует `README.md` в каталог `/home/` контейнера.

```bash
sudo docker exec -it lab_docker_single sh
ls -l /home
exit
```

Команды подключаются к контейнеру и проверяют наличие файла `README.md`.

```bash
sudo docker stop lab_docker_single
sudo docker rm lab_docker_single
```

Команды останавливают и удаляют контейнер.

## Проверка Docker Compose

```bash
sudo docker compose down -v
sudo docker compose up --build
```

Команды пересоздают контейнеры и запускают связку приложения с MySQL.

Проверка выполнялась в браузере:

```text
http://localhost:9888
```

Приложение успешно вывело данные из базы данных.

## Отправка на GitHub

```bash
git add .
git commit -m "complete docker laboratory work"
git push -u origin main
```

<img width="1280" height="800" alt="VirtualBox_Ubunta_09_06_2026_11_32_20" src="https://github.com/user-attachments/assets/d1a96f4a-3fc7-4af6-b680-abd57bd2878c" />

