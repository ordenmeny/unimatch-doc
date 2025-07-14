# Запуск Django-проекта локально

### 1. Клонируйте репозиторий:
```
git clone https://github.com/ordenmeny/UniMatch-django-backend.git
```
#### Перейдите в директорию проекта:
```
cd UniMatch-django-backend
```

### 2. Установите uv (пакетный менеджер python):
{% list tabs %}

- macOS или Linux
  ```
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```

- Windows (PowerShell)
  ```
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
  ```

{% endlist %}

### 3. Установите версию python:
```
uv python pin 3.12
```

{% note info %}

Можно узнать текущую версию python, открыв файл .python-version.

{% endnote %}

### 4. Установка зависимостей:
```
uv sync
```

{% note info %}

Для добавление нового пакета выполните команду:
> uv add <package_name>

{% endnote %}

### 5. Выполните миграции:
```
uv run python manage.py makemigrations
uv run python manage.py migrate
```

### 6. Создайте суперпользователя:
```
uv run python manage.py createsuperuser
```

### 7. Запуск сервера:
```
uv run python manage.py runserver
```
