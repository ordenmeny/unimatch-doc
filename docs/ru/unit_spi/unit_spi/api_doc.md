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

{% note info %}

organizer - пользователь, который создал мероприятие.
joined_users - все пользователи на мероприятие, в т.ч. и организатор.

Необходимо передать access_token в заголовки, явно указывать organizer и joined_users не нужно.

{% endnote %}

```http
  Authorization: Bearer <access_token>
```

Тело запроса при POST-запросе:
```json
{
    "title": "<str>",
    "time": "<str>",
    "location": "<str>",
    "description": "<str>",
    "tags": "<str>"
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
        {
            "id": 1,
            "email": "<str>",
            "chat_id": <int>,
            "tg_link": "<str>",
            "first_name": "<str>",
            "last_name": "<str>"
        },
        {
            "id": 2,
            "email": "<str>",
            "chat_id": <int>,
            "tg_link": "<str>",
            "first_name": "<str>",
            "last_name": "<str>"
        }
    ],
    "organizer": {
        "id": 1,
        "email": "<str>",
        "chat_id": <int>,
        "tg_link": "<str>",
        "first_name": "<str>",
        "last_name": "<str>"
    }
}
```

### **Регистрация пользователя**
Запрос:
```http
POST: api/users/
```

{% note info %}

На frontend необходимо проверить пароль1 и пароль2 на соответствие, а уже потом передавать в запрос.

{% endnote %}

```json
{
    "email": "<str>",
    "chat_id": <int>,
    "tg_link": "<str>",
    "first_name": "<str>",
    "last_name": "<str>",
    "password": "1234",
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

{% note alert %}

После создания пользователя необходимо авторизовать пользователя отдельным запросом.

{% endnote %}

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

{% note alert %}

Необходимо сохранить access токен на клиенте.

{% endnote %}


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



### **Получить конкретного пользователя по id (только для админа)**
```http
GET: api/users/1/
```

### **Получить всех пользователей (только для админа)**
```http
GET: api/users/
```

Возможные ошибки (статус 400):
```json
{
    "email": [
        "user with this email already exists."
    ]
}
```

### **Получить текущего пользователя**
```http
GET: api/users/me/
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

```http
  Authorization: Bearer <access_token>
```