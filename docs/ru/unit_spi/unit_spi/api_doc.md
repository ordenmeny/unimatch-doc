# API-документация

### **Получение всех мероприятий**
Запрос:
```http
  GET: api/events/
```

Ответ (статус 200):
```json
[
    {
        "id": 1,
        "title": "<str>",
        "time": "<str>",
        "location": "<str>",
        "description": "<str>",
        "tags": "<str>",
        "joined_users": [
            1
        ]
    },
    {
        "id": 2,
        "title": "<str>",
        "time": "<str>",
        "location": "<str>",
        "description": "<str>",
        "tags": "<str>",
        "joined_users": [
            1
        ]
    }
]
```

### **Получение конкретного мероприятия**
Запрос:
```http
  GET: api/events/1/
```

Ответ (статус 200):
```json
{
    "id": 1,
    "title": "<str>",
    "time": "<str>",
    "location": "<str>",
    "description": "<str>",
    "tags": "<str>",
    "joined_users": [
        1
    ]
}
```


### **Создание мероприятия**
```http
  POST: api/events/
```

Тело запроса при POST-запросе:
```json
{
    "title": "<str>",
    "time": "<str>",
    "location": "<str>",
    "description": "<str>",
    "tags": "<str>",
    "joined_users": [
        1
    ]
}
```

Ответ (статус 201):
```json
{
    "id": 4,
    "title": "<str>",
    "time": "<str>",
    "location": "<str>",
    "description": "<str>",
    "tags": "<str>",
    "joined_users": [
        1
    ]
}
```

Пример полей:
```json
{
    "id": 4
    "title": "Концерт Linkin Park",
    "time": "10 pm",
    "location": "Лос-Анджелес, hollywood hills",
    "description": "Концерт рок-группы Linkin Park",
    "tags": "#Концерты",
    "joined_users": [
        1, 2, 3
    ]
}
```

### **Создать пользователя**
Запрос:
```http
POST: api/users/
```
:
```json
{
    "email": "<str>",
    "chat_id": <int>,
    "tg_link": "<str>",
    "first_name": "<str>",
    "last_name": "<str>"
}
```

Ответ (статус 200):
```json
{
    "id": <int>,
    "email": "<str>",
    "chat_id": <int>,
    "tg_link": "<str>",
    "first_name": "<str>",
    "last_name": "<str>"
}
```

### **Авторизовать пользователя**

Запрос:
```http
POST: api/token/
```

```json
{
    "email": "<str>",
    "password": "<str>"
}
```

Ответ (статус 200):
```json
{
    "access": "<str>"
}
```

Ответ (статус 401), если email или пароль неверные:
```json
{
    "error": "invalid_data"
}
```


### **Обновлении refresh токена**

Условие, при котором необходимо сделать refresh - если такой ответ на запрос:
```json
{
    "error": "access_not_valid", 
    "step": "to_refresh"
}
```

{% endnote %}

Обновление refresh:
```http
POST: api/token/refresh/
```

Ответ (статус 200):
```json
{
    "access": "<str>"
}
```

Ответы (статус 401):
```json
{
    "error": "refresh_not_found", 
    "step": "to_login"
}
```
```json
{
    "error": "refresh_expired", 
    "step": "to_login"
}
```

### **Условие для редиректа на страницу авторизации**
Ответ на запрос:
```json
{
    "error": "access_missing", 
    "step": "to_login"
}
```

```json
{
    "error": "refresh_not_found", 
    "step": "to_login"
}
```

```json
{
    "error": "refresh_expired", 
    "step": "to_login"
}
```



### **Получить конкретного пользователя по id**
```http
GET: api/users/1/
```

### **Получить всех пользователей**
```http
GET: api/users/
```

Возможные ошибки:
```json
{
    "email": [
        "user with this email already exists."
    ]
}
```