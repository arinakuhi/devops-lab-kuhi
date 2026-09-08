# Laboratory Work №2 — CI/CD with GitHub Actions and Docker Hub

University: ITMO University
Faculty: FICT
Course: Введение в веб технологии
Year: 2026/2027
Group: 1
Author: Kuhi Arina

## Цель работы

Настроить автоматический CI/CD-процесс для Docker-приложения с использованием GitHub Actions и Docker Hub.

## Что было выполнено

В рамках лабораторной работы было создано простое веб-приложение на Flask, которое возвращает сообщение `Hello from Docker CI/CD!`.

Для приложения были подготовлены:

- `app.py` — Flask-приложение;
- `requirements.txt` — список Python-зависимостей;
- `Dockerfile` — инструкция для сборки Docker-образа;
- `.github/workflows/docker-build.yml` — GitHub Actions workflow для CI/CD.

Перед настройкой CI/CD Docker-образ был собран и проверен локально.

Образ был собран командой:

```bash
docker build -t lab2-flask .
```

Для локального запуска использовался проброс порта `5001` основной системы на порт `5000` внутри контейнера:

```bash
docker run -d -p 5001:5000 --name lab2-flask-container lab2-flask
```

Работа приложения была проверена по адресу:

```text
http://localhost:5001
```

Результат:

```text
Hello from Docker CI/CD!
```

## CI/CD

Для автоматизации сборки и публикации Docker-образа был настроен GitHub Actions workflow.

Workflow автоматически запускается при каждом `push` в ветку `main` и выполняет следующие этапы:

1. Получает исходный код репозитория.
2. Настраивает Docker Buildx.
3. Авторизуется в Docker Hub с использованием GitHub Secrets.
4. Собирает Docker-образ.
5. Публикует образ в Docker Hub.
6. Выполняет условный этап Deploy.

Для безопасной авторизации в GitHub Repository Secrets были добавлены:

- `DOCKER_USERNAME`;
- `DOCKER_PASSWORD`.

Секрет `DOCKER_PASSWORD` содержит Docker Hub Personal Access Token и не хранится непосредственно в исходном коде.

## Docker Hub

Собранный GitHub Actions Docker-образ автоматически публикуется в репозиторий:

[kukhi/my-flask-app](https://hub.docker.com/r/kukhi/my-flask-app)

Основной тег образа:

```text
kukhi/my-flask-app:latest
```

## Структура проекта

```text
2026_2027-introduction-in-web-tech-2-kuhi_a/
├── .github/
│   └── workflows/
│       └── docker-build.yml
├── app.py
├── Dockerfile
├── requirements.txt
└── README.md
```

## Результат

После выполнения `git push` в ветку `main` GitHub Actions автоматически собирает Docker-образ и публикует его в Docker Hub как `kukhi/my-flask-app:latest`.

Workflow был успешно выполнен со статусом `Success`.

## Отчёт

Полный отчёт по лабораторной работе находится в основном учебном репозитории:

[Lab 2 Report](https://github.com/arinakuhi/2026_2027-introduction-in-web-tech-1-kuhi_a/blob/main/lab2/lab2_report.md)
