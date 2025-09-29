# Создание бэкапа БД PostgreSQL для Яндекс.Диск

### 1. Подготовка Restic + rclone
```bash
apt update
apt install -y rclone restic
```

### 2. Настройка rclone для Яндекс.Диск
```bash
rclone config
```

n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> yandex
Storage> yandex
client_id>
client_secret>
Edit advanced config? (y/n)
y) Yes
n) No (default)
y/n> n
Use auto config?
y/n> n
При просьбе ввода пароля (Enter NEW configuration password) придумать пароль.

### 3. Установить rclone на рабочий компьютер
### 4. Ввести команду в терминале для авторизации и пройти её:
```bash
rclone authorize "yandex"
```
### 5. Будет выдан token в таком виде:
```bash
{"access_token":"...","token_type":"bearer","refresh_token":"...","expiry":"2026-09-28T20:45:04.087213+05:00"}
```
### 6. Ввести токен на сервере и завершить последним подтверждением:
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> y

### 7. Проверить, что всё получилось:
```bash
rclone lsd yandex
```

### 8. Инициализация репозитория Restic
```bash
restic -r rclone:yandex:/db_backups init
```
Будет создан репозиторий db_backups в Яндекс.Диске
Для удобства зафиксируйте пароль в переменной окружения:
```bash
export RESTIC_PASSWORD="<секретный_пароль>"
```

### 9. Создать скрипт (backup.sh для бэкапа PostgreSQL в любом удобном месте на сервере:
```bash
#!/bin/bash
set -e

# Конфиг
export PGPASSWORD="your_db_password"
DB_USER="devuser"
DB_NAME="devdb"
CONTAINER="postgres_container"   # имя контейнера PostgreSQL
BACKUP_FILE="/tmp/dump_$(date +%Y-%m-%d_%H-%M-%S).sql"

export RESTIC_PASSWORD="your_restic_password"
RESTIC_REPO="rclone:yandex:/db_backups"

# Делаем дамп
docker exec -i $CONTAINER pg_dump -U $DB_USER $DB_NAME > $BACKUP_FILE

# Отправляем в restic
restic -r $RESTIC_REPO backup $BACKUP_FILE

# Удаляем локальный файл
rm $BACKUP_FILE
```

### 10. Сделай исполняемым:
```bash
chmod +x backup_postgres.sh
```

### 11. Запусить скрипт:
```bash
./backup.sh
```

### 12. Восстановление:
```bash
restic -r rclone:yandex:/db_backups snapshots   # список бэкапов
restic -r rclone:yandex:/db_backups restore latest --target /tmp/restore
```
Файл будет в /tmp/restore/....
Потом:
```bash
docker exec -i postgres_container psql -U devuser -d devdb < /tmp/restore/dump_xxx.sql
```
