# Запуск проекта на сервере через docker и git-submodules

### 1. Склонировать проект:
```bash
git clone --recurse-submodules https://github.com/ordenmeny/UniMatch.git
```

### 2. В директории UniMatch создать файл .env:
```txt
DB_NAME=<devdb>
DB_USER=<devuser>
DB_PASS=<devpassword>
DB_HOST=<db>
SECRET_KEY=<secret_key>
DEBUG=False
TOKEN=<token>
FRONTEND_HOST="https://unimatch.ru"
YANDEX_CLIENT_ID=<yandex_cliend_id>
YANDEX_CLIENT_SECRET=<yandex_client_secret>
SECURE_HTTP_ONLY=True
DOCKER_PROJECT=True
```

### 3. Запустить проект через docker
```bash
docker compose up --build -d
```
### 4. Если внести изменения в репозиторий backend:
1. Вносим изменения, делаем коммит, делаем пуш.
2. Вносим изменения в основном репозиторий:
```
git add djangoProject
git commit -m "changes djangoProject"
git push
```
3. На сервере стягиваем измнения:
```bash
git pull --recurse-submodule
```

## Если нужно добавить новый репозиторий в общий:
```bash
git submodule add <URL-репозитория> <путь-для-размещения>
```

## Для удобства можно создать alias.
```bash
nano ~/.bashrc
```
Внести изменения в .bashrc:
```bash
alias git-pull-submod="git pull --recurse-submodule"
```
Сохранить изменения:
```bash
source ~/.bashrc
```
