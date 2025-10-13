University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)  
Year: 2025/2026  
Group: U4225  
Author: Shtof Fedor Danilovich  
Lab: Lab2  
Date of create: 07.10.2025  
Date of finished: 10.10.2025  

---

# Лабораторная работа №2  
## Тема: CI/CD для Docker-приложения

---

### 🎯 Цель работы  
Освоить базовые принципы настройки непрерывной интеграции и доставки (CI/CD) для Docker-приложения с использованием GitHub Actions и Docker Hub.  
Реализовать автоматическую сборку, публикацию и деплой Docker-образа при каждом изменении в коде репозитория.

---

### 🧩 Ход выполнения

#### 1. Подготовка проекта  
Из репозитория лабораторной работы №1 были скопированы файлы приложения:
- `app.py` — основной исполняемый файл Flask-приложения;  
- `requirements.txt` — список зависимостей для установки;  
- `Dockerfile` — инструкция для сборки Docker-образа.  

Создан новый репозиторий под лабораторную работу №2.  
Добавлены файлы в корень проекта и загружены на GitHub.

---

#### 2. Настройка Docker Hub  
- Зарегистрирован аккаунт на [Docker Hub](https://hub.docker.com).  
- Создан новый публичный репозиторий с именем `my-flask-app`.  
- Сгенерирован **Access Token** (с правами `Read` и `Write`), который был сохранён в настройках GitHub Actions как секрет.  

Добавленные секреты:
- `DOCKERHUB_USERNAME` — имя пользователя Docker Hub;  
- `DOCKERHUB_TOKEN` — токен доступа для аутентификации при публикации образа.

---

#### 3. Создание GitHub Actions workflow  
В корне проекта создана папка `.github/workflows/`, где добавлен файл `docker-build.yml`.  
В нём настроен пайплайн для автоматической сборки и публикации Docker-образа при каждом пуше в ветку `main`.

Содержимое workflow:

```yaml
name: Docker CI/CD

on:
  push:
    branches: ["main"]

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/my-flask-app:latest

      - name: Deploy (placeholder)
        run: echo "Deploy step executed successfully."

4. Тестирование CI/CD пайплайна

Для проверки работы пайплайна был выполнен коммит в ветку main.
После этого во вкладке Actions запустился workflow Docker CI/CD.

В логах были успешно выполнены шаги:
	•	Checkout code — репозиторий успешно загружен;
	•	Set up Docker Buildx — сборочная среда настроена;
	•	Log in to Docker Hub — выполнен вход с использованием секретов;
	•	Build and push Docker image — образ собран и загружен в Docker Hub;
	•	Deploy (placeholder) — завершение пайплайна.

6. Вывод
В ходе лабораторной работы изучены основы организации CI/CD-процессов на платформе GitHub Actions.
Настроен полный цикл автоматизации — от сборки Docker-образа до его публикации в Docker Hub.
Полученные знания позволяют применять практики DevOps для ускорения и упрощения процесса разработки, тестирования и доставки приложений.
Результатом стало готовое CI/CD-решение, пригодное для масштабирования и интеграции в будущие проекты.

⸻

