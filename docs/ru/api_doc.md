# API-документация

### **Обработка ошибок с кодом 401 и обновление access токена**

{% note info "" %}

Ошибка с кодом 401 означает, что JWT-токен недействителен, поэтому его требуется обновить.

{% endnote %}

### Сделать запрос на обновление access-токена
```http
POST: api/token/refresh/
```
* Если получен новый access токен и код ответа 200 - сохраняем access токен.
Ответ (статус 200):
```json
{
    "access": "<access_token>",
}
```
* Если код ответа 401 - редирект на страницу входа и вывод сообщения error пользователю.
Ответ (статус 401):
```json
{
  "error": "Необходимо пройти авторизацию",
}
```

### **Регистрация**
Запрос:
```http
  POST api/register/
```

```json
{
  "email": "почта@example.com",
  "first_name": "Имя",
  "last_name": "Фамилия",
  "birth": "2000-01-28",
  "password": "пароль",
  "hobby": [1, 2, 5]
}
```

{% note info "Уточнение" %}

[1, 2, 5] - индексы хобби (тэгов)

{% endnote %}

Ответ (статус 201):

```json
  {
    "user": {
        "first_name": "Имя",
        "last_name": "Фамилия",
        "email": "email@example.com",
        "birth": "2000-01-28",
        "chat_id": null,
        "image": null,
        "hobby": [
            {
                "id": 1,
                "name": "Программирование"
            },
            {
                "id": 2,
                "name": "Спорт"
            }],
    },
    "access": "<access_token>"
}
```

{% note alert %}

Необходимо сохранить access токен на клиенте.

{% endnote %}

Ответ (статус 400) - возможные ошибки:

```json
{
    "error": "Пользователь с таким email уже существует"
}
```

```json
{
    "error": "Введите правильный адрес электронной почты"
}
```

```json
{
    "error": "Пароль либо слишком простой, либо содержит меньше 4 символов"
}
```

```json
{
    "error": "Проблема с полем ввода даты рождения"
}
```

```json
{
    "error": "Произошла непредвиденная ошибка"
}
```

{% note info "" %}

Ошибки выводятся по отдельности.

{% endnote %}

{% note alert %}

На клиенте проверить пароли на совпадение и уже только после передавать в запрос.

{% endnote %}

{% note info "Желательно" %}

Проверить валидность email, first_name, last_name, birth на формат.

{% endnote %}

### **Авторизация**
Запрос:
```http
  POST api/token/
```

```json
{
  "email": "user9@yandex.ru",
  "password": "пароль"
}
```

Ответ (статус 200):

```json
{
  "access": "<access_token>"
}
```

{% note alert %}

Необходимо сохранить access токен на клиенте.

{% endnote %}

Ответ (статус 400):

```http
{
  "error": "Неверный email или пароль"
}
```

### **Получение текущего пользователя по JWT-токену**
Запрос:
```http
  GET: api/auth/users/me/
```

```http
  Authorization: Bearer <access_token>
```

Ответ (статус 200):
```
{
    "first_name": "Имя",
    "last_name": "Фамилия",
    "email": "user9@yandex.ru",
    "birth": "2000-08-12",
    "chat_id": null,
    "image": null,
    "hobby": [
        {
            "id": 1,
            "name": "Программирование"
        },
        {
            "id": 2,
            "name": "Спорт"
        }
    ]
}
```

Ответ (статус 401):
```json
{
    "detail": "Given token not valid for any token type",
    "code": "token_not_valid",
    "messages": [
        {
            "token_class": "AccessToken",
            "token_type": "access",
            "message": "Token is expired"
        }
    ]
}
```

### **Выйти**
{% note alert %}

Необходимо удалить access токен на клиенте (вне зависимости от ответа бэкэнда) и сделать запрос на backend для отзыва refresh-токена.

{% endnote %}

Запрос:
```http
POST: api/token/blacklist/
```

Ответ (статус 200):
```json
{
  "signout": "Вы вышли из системы"
}
```

Ответ (статус 400):
```json
{
  "error": "Не получилось выйти из системы"
}
```

### **Получение истории мэтчей текущего пользователя**
Запрос:
```http
  GET: api/pairs/
```

```http
  Authorization: Bearer <access_token>
```

Ответ (статус 200):
* current_pair - текущая пара пользователя.
* pairs - предыдущие пары пользователя.

```json
{
    "current_pair": {
        "id": 228,
        "partner": {
            "id": 45,
            "first_name": "Имя",
            "last_name": "Фамилия",
            "email": "email@example.com",
            "hobby": [
                {
                    "id": 1,
                    "name": "Программирование"
                },
                {
                    "id": 2,
                    "name": "Спорт"
                },
                {
                    "id": 5,
                    "name": "Рисование"
                }
            ]
        },
        "created_at": "2025-05-08"
    },
    "pairs": [
        {
            "id": 225,
            "partner": {
                "id": 31,
                "first_name": "Дмитрий",
                "last_name": "Александров",
                "email": "admin@yandex.ru",
                "hobby": [
                    {
                        "id": 1,
                        "name": "Программирование"
                    },
                    {
                        "id": 2,
                        "name": "Спорт"
                    },
                    {
                        "id": 7,
                        "name": "История"
                    }
                ]
            },
            "created_at": "2025-05-08"
        },
        {
            "id": 226,
            "partner": {
                "id": 58,
                "first_name": "Дэн",
                "last_name": "Рейнолдс",
                "email": "danraynolds15@example.com",
                "hobby": [
                    {
                        "id": 1,
                        "name": "Программирование"
                    },
                    {
                        "id": 2,
                        "name": "Спорт"
                    },
                    {
                        "id": 5,
                        "name": "Рисование"
                    }
                ]
            },
            "created_at": "2025-05-08"
        }
    ]
}
```

### **Получение списка всех тэгов**
Запрос:
```http
  GET: api/hobby/all/
```

Ответ (статус 200):

```json
[
    {
        "id": 1,
        "name": "Программирование"
    },
    {
        "id": 2,
        "name": "Спорт"
    },
    {
        "id": 3,
        "name": "Рисование"
    },
    {
        "id": 4,
        "name": "Литература"
    }
]
```

### **Получение тэгов пользователя и всех тэгов в одном запросе**
Запрос:
```http
GET: api/hobby/total/
```

```http
Authorization: Bearer <access_token>
```

Ответ (статус 200):

```json
{
    "user_tags": [
        {
            "id": 2,
            "name": "Литература"
        },
        {
            "id": 3,
            "name": "История"
        }
    ],
    "all_tags": [
        {
            "id": 1,
            "name": "Программирование"
        },
        {
            "id": 2,
            "name": "Литература"
        },
        {
            "id": 3,
            "name": "История"
        }
    ]
}
```

### **Дней до мэтча**
Запрос:
```http
GET: api/days-to-match/
```

Ответ (статус 200):

```json
{
    "days_left": 1, // полных дней до
    "hours_left": 13, // полных дней до + полных часов до
    "minutes_left": 17, // полных дней до + полных часов до + полных минут до
    "next_match_at": "2025-04-29 10:00:00" // В общем виде дата-время след.мэтча
}
```

### **Обновление данных пользователя (в т.ч хобби)**
Запрос:
```http
PATCH api/update/user/
```

```http
Authorization: Bearer <access_token>
```

{% note info %}

Можно передать только те данные, которые были изменены.

{% endnote %}

```json
{
  "first_name": "Дэн",
  "last_name": "Рейнолдс",
  "birth": "2000-01-28",
  "hobby": [1, 2, 7],
  "tg_link": "@user-dan"
}
```

Ответ (статус 200):

```json
{
    "id": 59,
    "first_name": "Дэн",
    "last_name": "Рейнолдс",
    "email": "danraynolds16@example.com",
    "birth": "2000-01-28",
    "chat_id": null,
    "image": null,
    "hobby": [
        1,
        2,
        7
    ],
    "tg_link": "@user-dan"
}
```
