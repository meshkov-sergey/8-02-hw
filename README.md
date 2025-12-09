# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Мешков Сергей`

---

## Задание 1

`Установите Zabbix Server с веб-интерфейсом.`

Cкриншот авторизации в админке
![скриншот авторизации в админке](https://github.com/meshkov-sergey/8-02-hw/blob/main/img/zabbix-auth.png)

### Установка PostgreSQL
sudo apt update
sudo apt install -y postgresql postgresql-contrib

### Создание БД для Zabbix
sudo -u postgres psql -c "CREATE USER zabbix WITH PASSWORD '123456789';"
sudo -u postgres psql -c "CREATE DATABASE zabbix OWNER zabbix;"

### Установка репозитория Zabbix
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
sudo apt update

### Установка Zabbix компонентов
sudo apt install -y zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent

### Импорт схемы БД
sudo zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

### Настройка пароля БД
sudo nano /etc/zabbix/zabbix_server.conf
### DBPassword=123456789

### Настройка Nginx
sudo nano /etc/zabbix/nginx.conf
### listen 80;

### Добавление конфига Zabbix в основной конфиг
sudo nano /etc/nginx/nginx.conf
### /etc/zabbix/nginx.conf;

### Удаление конфликтующих конфигов
sudo rm -f /etc/nginx/sites-enabled/default

### Запуск сервисов
sudo systemctl restart zabbix-server zabbix-agent nginx php8.3-fpm
sudo systemctl enable zabbix-server zabbix-agent nginx php8.3-fpm

### Задание 2

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6.

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


---

### Задание 3

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6.

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`

### Задание 4

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6.

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
