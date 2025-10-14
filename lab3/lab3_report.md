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

### 🧩 Ход выполнения

#### 1. Подготовка окружения
Были установлены и запущены контейнеры **Prometheus**, **Node Exporter** и **Grafana** с использованием Docker.  
Создана отдельная сеть `monitoring`, в которой все сервисы взаимодействуют между собой:
```bash
docker network create monitoring
```
- **Проверена успешность создания сети:**

  ```bash
  docker network ls<img width="1440" height="900" alt="1" src="https://github.com/user-attachments/assets/0a247e58-1b73-46ef-bf26-5ad04a88b43d" />
   ``` 
Все сервисы (Prometheus, Node Exporter, Grafana) будут подключены к этой сети.
<img width="1440" height="900" alt="2" src="https://github.com/user-attachments/assets/bf7837b6-8308-44a1-b640-5f38d1f5d2fb" />
#### 2. Настройка Prometheus
Создана папка prometheus/ и в ней файл prometheus.yml:
```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["prometheus:9090"]

  - job_name: "node-exporter"
    static_configs:
      - targets: ["node-exporter:9100"]
```
Создан том для данных Prometheus
Запущен контейнер Prometheus:
```
docker run -d \
  --name prometheus \
  --restart=unless-stopped \
  --network monitoring \
  -p 9090:9090 \
  -v prometheus-data:/prometheus \
  -v "$(pwd)"/prometheus:/etc/prometheus \
  prom/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/prometheus \
  --storage.tsdb.retention.time=200h \
  --web.enable-lifecycle
```
<img width="1440" height="900" alt="5" src="https://github.com/user-attachments/assets/b42e000d-f28d-429b-9ca9-3b72110f11db" />

#### 3. Запуск Node Exporter
```
docker run -d \
  --name node-exporter \
  --restart=unless-stopped \
  --network monitoring \
  -p 9100:9100 \
  -v "/proc:/host/proc:ro" \
  -v "/sys:/host/sys:ro" \
  -v "/:/rootfs:ro" \
  prom/node-exporter \
  --path.procfs=/host/proc \
  --path.rootfs=/rootfs \
  --path.sysfs=/host/sys \
  --collector.filesystem.mount-points-exclude="^/(sys|proc|dev|host|etc)($$|/)"
```
<img width="1440" height="900" alt="6" src="https://github.com/user-attachments/assets/36ead23f-0787-4792-b98d-73b406ad7498" />
#### 4. Запуск Grafana
Создание тома:
```
docker volume create grafana-data
```
Запуск контейнера Grafana:
```
docker run -d \
  --name grafana \
  --restart=unless-stopped \
  --network monitoring \
  -p 3000:3000 \
  -v grafana-data:/var/lib/grafana \
  -e "GF_SECURITY_ADMIN_PASSWORD=admin" \
  grafana/grafana
```
#### 5. Настройка Grafana

<img width="1440" height="900" alt="3" src="https://github.com/user-attachments/assets/7f3ed6ab-23cc-46a0-89d3-53c37a3b69d8" />

#### 6. Создание дашборда

<img width="1440" height="900" alt="4" src="https://github.com/user-attachments/assets/8b7e6b79-5210-41b4-aa5e-5825edb6720b" />





<img width="1440" height="900" alt="7" src="https://github.com/user-attachments/assets/8e60853a-0061-4ce8-8905-9a7f5221d108" />
