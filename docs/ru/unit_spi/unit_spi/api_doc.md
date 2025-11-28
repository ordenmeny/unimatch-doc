# API-документация

### **Получение всех мероприятий**
Запрос:
```http
  GET: api/events/
```

Ответ (статус 200):
```
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
```
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
```
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
```
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
```
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