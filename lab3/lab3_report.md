University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)  
Year: 2025/2026  
Group: U4225  
Author: Shtof Fedor Danilovich  
Lab: Lab3  
Date of create: 10.10.2025  
Date of finished: 14.10.2025  

---

# Лабораторная работа №3  
## Тема: Мониторинг с Prometheus и Grafana

---

### 🎯 Цель работы  
Научиться настраивать систему мониторинга с использованием Prometheus для сбора метрик и Grafana для их визуализации.  
Освоить принципы работы экспортеров, настройку конфигурационных файлов и создание дашбордов для анализа состояния системы.

---

### 🧩 Ход выпо<img width="1440" height="900" alt="Снимок экрана 2025-10-13 в 20 03 46" src="https://github.com/user-attachments/assets/a722158a-4d3a-4d74-82e1-95a6b9e66d48" />
лнения

#### 1. Подготовка окружения
Были установлены и запущены контейнеры **Prometheus**, **Node Exporter** и **Grafana** с использованием Docker.  
Создана отдельная сеть `monitoring`, в которой все сервисы взаимодействуют между собой:
Запущены необходимые порты:
<img width="1440" height="900" alt="1" src="https://github.com/user-attachments/assets/0a247e58-1b73-46ef-bf26-5ad04a88b43d" />
