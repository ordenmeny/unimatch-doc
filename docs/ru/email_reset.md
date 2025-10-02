# Сброс пароля по почте

### 1. На frontend пользователь нажимает на кнопку "Сбросить пароль".

### 2. Отправляется запрос на backend:
```json
  POST: api/auth/users/reset_password/
```
```json
{
  "email": "users_email@gmail.com"
}
```

Возможные статусы ответа:
[`204 No Content`](#code) - запрос выполнен успешно, инструкции по сбросу пароля отправлены на email.
[`400 Bad Request`](#code) - неверные данные в запросе или пользователь не найден.

Backnend отправляет письмо со сбросом пароля на почту:

```txt
  ...
  Пожалуйста, перейдите на эту страницу и введите новый пароль:
  https://unimatch.ru/password/reset/confirm/MTM0/c41ddsd5djdjdqnqnd825c9fe575
  Ваше имя пользователя, если вы забыли: planeairbus380@gmail.com
  ...
```
```txt
  Шаблон ссылки: password/reset/confirm/<uid>/<token>
```

### 3. Пользователь переходит по ссылке и попадает на frontend, где вводит новый пароль.
После этого необходимо сделать запрос:
```json
  POST: api/auth/users/reset_password_confirm/
```

```json
{
    "uid": "<uid>",
    "token": "<token>",
    "new_password": "<new_password>"
}
```

Возможные статусы ответа:
[`204 No Content`](#code) - пароль успешно изменен.
[`400 Bad Request`](#code) - неверный или просроченный токен, либо некорректные данные.
