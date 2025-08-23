# Документация по пользовательской системе

## Настройки DJOSER
```
TOKEN_MODEL=None — не используем стандартные токены (authtoken) и работаем с JWT (Simple JWT).

LOGIN_FIELD='email' — логинимся по email

SERIALIZERS.user/current_user — какой сериализатор возвращать для /users/ и /users/me/
```


## Настройки SIMPLE_JWT
```
ACCESS_TOKEN_LIFETIME - время жизни access токена (15 минут)

REFRESH_TOKEN_LIFETIME - время жизнм refresh токена (1 день)

ROTATE_REFRESH_TOKENS - при refresh выдаётся новый refresh

BLACKLIST_AFTER_ROTATION - старый refresh уходит в blacklist

TOKEN_OBTAIN_SERIALIZER - указывает на сериализатор входа (по email)
```
