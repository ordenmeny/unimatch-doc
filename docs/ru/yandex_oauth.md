# Документация по яндекс oauth

## Схема работы авторизации/регистрации пользователя через яндекс oauth


### 1. На frontend делается запрос для получения ссылки на сервис яндекса.
```
GET: api/auth/get_yandex_auth_url/
```

### 2. Пользователь переходит на сервис яндекса по сформированной ссылке.
Пример страницы с кнопкой (React):
```javascript
import { useState } from "react";

export default function Home() {
  const [loading, setLoading] = useState(false);

  const handleLogin = async () => {
    setLoading(true);
    try {
      const response = await fetch(
        "https://backend.unimatch.ru/api/auth/get_yandex_auth_url/",
      );
      const data = await response.json();
      // Перенаправляем пользователя на страницу авторизации Яндекс
      window.location.assign(data.auth_url);
    } catch (error) {
      console.error("Ошибка при получении ссылки:", error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>Войти с Яндекс ID</h1>
      <button onClick={handleLogin} disabled={loading}>
        {loading ? "Загрузка..." : "Войти с Яндекс ID"}
      </button>
    </div>
  );
}
```


### 3. Пользователь вводит данные своего яндекс-аккаунта.
### 4. Сервис яндекса делает редирект на backend с указанием кода в query-параметрах.
<!--Редирект: /auth/yandex/verification_code?code=<code>.-->

{% note warning %}

Редирект происходит на адрес, указанный в **[настройках яндекс-oauth](https://oauth.yandex.ru)**.

{% endnote %}

### 5. На бэкэнде происходит обмен кода на OAuth-токен яндекса.
### 6. Делается запрос на получение данных пользователя, в т.ч. почты.
### 7. На основе почты происходит аутентицикация или регистрация пользователя.
### 8. Создаются access и refresh токены пользователя сайта.
### 9. Токены заносятся в httponly куки сайта.
{% note info %}

Для правильной и безопасной работы куков обратить внимание на следующие настройки backend:

```python
frontend_host = "frontend.unimatch.ru"
CORS_ALLOW_CREDENTIALS = ...
CORS_ALLOWED_ORIGINS = [
    f"https://{frontend_host}",
]

# === CSRF ===
CSRF_TRUSTED_ORIGINS = [
    f"https://{frontend_host}",
]

SESSION_COOKIE_SAMESITE = ...
SESSION_COOKIE_SECURE = ...

CSRF_COOKIE_SAMESITE = ...
CSRF_COOKIE_SECURE = ...
```

Также обратить внимание на установку HttpOnly куков (на backend):
```python
response.set_cookie(
    key="access_token",
    value=access_token,
    httponly=True,
    secure=settings.SECURE_HTTP_ONLY,
    samesite="Lax",  # Здесь может потребоваться другой параметр.
    max_age=24 * 60 * 60,
    path="/",
    # Если frontend и backend на разных доменах, может потребоваться настройка:
    domain="..."
)
```
{% endnote %}



### 10. Происходит редирект на frontend.
### 11. На странице делается запрос для получения access токена.
```json
GET: api/access/httponly/
```

Ответ (статус 200):
```json
{
  "access": <access_token>
}
```
#### Пример на React:
```javascript
import { useEffect, useState } from "react";
import { useNavigate } from "react-router-dom";

export default function Me() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const navigate = useNavigate();

  useEffect(() => {
    const fetchUser = async () => {
      try {
        const response = await fetch(
          "https://backend.unimatch.ru/api/access/httponly/",
          {
            credentials: "include", // <-- обязательно для HttpOnly cookies!
          },
        );

        if (response.status === 401) {
          navigate("/");
          return;
        }

        const data = await response.json();
        setUser(data);
      } catch (error) {
        console.error("Ошибка загрузки пользователя:", error);
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [navigate]);

  if (loading) return <p>Загрузка...</p>;
  if (!user) return <p>Пользователь не найден</p>;

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <p>access токен: {user.access}</p>
    </div>
  );
}

```

### 12. На frontend необходимо сохранить access токен для последующего использования.
Пример:
```json
GET: api/auth/users/me/
```
```json
Authorization: Bearer <access_token>
```

Ответ (статус 200):
```json
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

## Обработка ошибок
### Статус 503:
```json
{
  "error": "Сервис авторизации недоступен"
}
```

### Статус 400:
```json
{
  "error": "Не удалось получить токен пользователя яндекса"
}
```

```json
{
  "error": "Произошла ошибка получения почты пользователя"
}
```

```json
{
  "error": "Чтобы войти, используйте почту"
}
```

{% note info "Рекоментация" %}

Очистить куки от access токена и вернуть пользователя на главную страницу авторизации с сообщением "недоступен сервис авторизации яндекс". А в случае с ошибкой получения почты - с сообщением "ваша яндекс-почта недоступна".

{% endnote %}
