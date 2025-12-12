# Домашнее задание к занятию "8.2. Система мониторинга Zabbix" - Мешков Сергей

---

## Задание 1

**Установите Zabbix Server с веб-интерфейсом.**

### Выполненные этапы:

1. **Установка и настройка PostgreSQL:**
   - Установка PostgreSQL и дополнительных модулей
   - Создание пользователя и базы данных для Zabbix

2. **Установка Zabbix Server:**
   - Добавление официального репозитория Zabbix для Ubuntu 24.04
   - Установка всех необходимых компонентов: Zabbix Server, веб-интерфейс, агент

3. **Настройка базы данных:**
   - Импорт начальной схемы базы данных
   - Настройка подключения к БД в конфигурационном файле

4. **Настройка веб-сервера Nginx:**
   - Конфигурация Nginx для работы с Zabbix Frontend
   - Решение конфликтов портов с другими сервисами

5. **Запуск и проверка:**
   - Запуск всех необходимых служб
   - Проверка доступности веб-интерфейса
   - Вход в административную панель Zabbix

### Использованные команды:

```bash
# Установка PostgreSQL
sudo apt update
sudo apt install -y postgresql postgresql-contrib

# Создание БД для Zabbix
sudo -u postgres psql -c "CREATE USER zabbix WITH PASSWORD '123456789';"
sudo -u postgres psql -c "CREATE DATABASE zabbix OWNER zabbix;"

# Установка репозитория Zabbix
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
sudo apt update

# Установка Zabbix компонентов
sudo apt install -y zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent

# Импорт схемы БД
sudo zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# Настройка пароля БД
sudo nano /etc/zabbix/zabbix_server.conf
# DBPassword=123456789

# Настройка Nginx
sudo nano /etc/zabbix/nginx.conf
# listen 80;

# Добавление конфига Zabbix в основной конфиг
sudo nano /etc/nginx/nginx.conf
# include /etc/zabbix/nginx.conf;

# Удаление конфликтующих конфигов
sudo rm -f /etc/nginx/sites-enabled/default

# Запуск сервисов
sudo systemctl restart zabbix-server zabbix-agent nginx php8.3-fpm
sudo systemctl enable zabbix-server zabbix-agent nginx php8.3-fpm
```

### Скриншот авторизации в админке:
![скриншот авторизации в админке](https://github.com/meshkov-sergey/8-02-hw/main/img/zabbix-auth.png)


## Задание 2

**Установите Zabbix Agent на два хоста.**

### Выполненные этапы:

1. **Установка Zabbix Agent на локальную VM** (где установлен Zabbix Server):
   - Агент установлен как системная служба
   - Настроено подключение к Zabbix Server по адресу `127.0.0.1:10051` и `10.0.2.15:10051`
   - Хост добавлен в веб-интерфейс с именем `Zabbix-serv-Agent`

2. **Установка Zabbix Agent в Docker контейнере** (второй хост):
   - Запущен Docker контейнер с образом `zabbix/zabbix-agent:ubuntu-7.4-latest`
   - Контейнер настроен на подключение к Zabbix Server (`10.0.2.15:10051`)
   - Используется порт `10052` для избежания конфликтов с локальным агентом
   - Хост добавлен в веб-интерфейс с именем `Docker-Agent-1`

3. **Настройка в веб-интерфейсе Zabbix:**
   - Оба агента добавлены в раздел **Data collection > Hosts**
   - Настроены интерфейсы с правильными IP и портами
   - Применён шаблон **Linux by Zabbix agent** для обоих хостов

4. **Проверка работоспособности:**
   - Агенты успешно подключаются к серверу (видны в логах)
   - Данные поступают в раздел **Monitoring > Latest data**
   - Обнаружены и отображаются проблемы (low disk space в Docker контейнере)

### Использованные команды:

```bash
# Настройка локального агента
sudo nano /etc/zabbix/zabbix_agentd.conf
# Server=127.0.0.1,10.0.2.15
# ServerActive=127.0.0.1,10.0.2.15
# Hostname=Zabbix-serv-Agent
sudo systemctl restart zabbix-agent

# Запуск Zabbix Agent в Docker контейнере
docker run -d \
  --name zabbix-agent-docker \
  -e ZBX_HOSTNAME="Docker-Agent-1" \
  -e ZBX_SERVER_HOST="10.0.2.15" \
  -e ZBX_SERVER_PORT="10051" \
  -p 10052:10050 \
  --restart unless-stopped \
  zabbix/zabbix-agent:ubuntu-7.4-latest
```

### Скриншоты:

![Configuration Hosts](https://github.com/meshkov-sergey/8-02-hw/main/img/zabbix-configuration-hosts.png) - Раздел Hosts с подключенными агентами

![Local Agent Logs](https://github.com/meshkov-sergey/8-02-hw/main/img/zabbix-agent-logs-local.png) - Логи локального агента (подключение к серверу)

![Docker Agent Logs](https://github.com/meshkov-sergey/8-02-hw/main/img/zabbix-agent-logs-docker.png) - Логи Docker агента (подключение к серверу)

![Latest Data](https://github.com/meshkov-sergey/8-02-hw/main/img/zabbix-monitoring-latest-data.png) - Latest data с данными от обоих агентов
