# Авторизация через Telegram
## Схема работы

### 1. На странице подключается официальный Telegram Login Widget.
### 2. Когда пользователь нажимает на кнопку «Войти через Telegram», виджет вызывает глобальную функцию onTelegramAuth(user).
### 3. В объекте user приходят данные пользователя из Telegram (id, username, first_name, hash и др.).
### 4. Эти данные отправляются на backend.
Запрос:
```http
POST: api/tg-auth/
```
### 5. Backend проверяет подлинность данных и возвращает результат авторизации:
```python
def check_telegram_auth(data: dict, bot_token: str) -> bool:
    data = data.copy()
    check_hash = data.pop('hash')

    sorted_data = sorted([f"{k}={v}" for k, v in data.items()])

    # Приводим данные (username, id, first_name, auth_date) к формату.
    data_string = '\n'.join(sorted_data)

    # Токен бота хэшируется алгоритмом sha256 и
    # возвращается в бинарном виде.
    secret_key = hashlib.sha256(bot_token.encode()).digest()

    # Получаем HMAC через алгоритм sha256 на основе полученных secret_key, данных пользователя.
    # Возващаем в hex-виде (16-ричное представление).
    hmac_hash = hmac.new(secret_key, data_string.encode(), hashlib.sha256).hexdigest()

    # Если хэш, который прислал телеграм, равен полученному, данные подделаны не были.
    return hmac_hash == check_hash
```

Документация **[Телеграм](https://core.telegram.org/widgets/login?ysclid=mfvgmm3wcb289397888)**

### 6. Backend возващает ответ.
Ответ (статус 200):
```json
{
  "access": <access_token>
}
```

Ответ (статус 400) - ошибка:
```json
{
  "error": "Invalid tg auth"
}
```

### 7. Frontend получает ответ в виде access токена.
{% note warning %}

Необходимо сохранить access токен на клиенте.

{% endnote %}

## Пример реализации на React JS:
```javascript
import { useEffect } from "react";

export default function Home() {
  useEffect(() => {
    // Создаём колбэк глобально (виджет будет искать его в window)
    window.onTelegramAuth = function (user) {
      console.log("TG user:", user);

      // Делаем запрос на backend
      fetch(
        "https://6a34762ec2b18127f6e20f8e5f49296c.serveo.net/api/tg-auth/",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(user),
        },
      )
        .then((res) => {
          if (!res.ok) {
            throw new Error("Ошибка авторизации");
          }
          return res.json();
        })
        .then((data) => {
          console.log("Ответ от backend:", data);
          alert("Добро пожаловать, " + data.access);
        })
        .catch((err) => {
          console.error(err);
          alert("Ошибка при авторизации");
        });
    };

    // Подключаем сам скрипт Telegram
    const script = document.createElement("script");
    script.src = "https://telegram.org/js/telegram-widget.js?22";
    script.async = true;
    script.setAttribute("data-telegram-login", "Uni_Match_Bot");
    script.setAttribute("data-size", "large");
    script.setAttribute("data-userpic", "false");
    script.setAttribute("data-radius", "14");
    script.setAttribute("data-request-access", "write");
    script.setAttribute("data-onauth", "onTelegramAuth(user)");

    document.getElementById("telegram-login-container").appendChild(script);

    return () => {
      // Чистим при размонтировании
      const container = document.getElementById("telegram-login-container");
      if (container) container.innerHTML = "";
    };
  }, []);

  return (
    <div>
      <h1>Главная страница</h1>
      <div id="telegram-login-container"></div>
    </div>
  );
}
````
