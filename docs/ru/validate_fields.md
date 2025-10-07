# Валидация полей

### Валидация email:
```javascript
function isValidEmail(value) {
  // Базовая проверка по принципу Django EmailValidator
  const re = /^[^@\s]+@[^@\s]+\.[^@\s]+$/;
  return re.test(value);
}

console.log(isValidEmail("test@example.com")); // true
console.log(isValidEmail("bad@address"));      // false
console.log(isValidEmail("имя@домен.рф"));     // true (допустимы Unicode-домены)
```

### Валидация username (в данном проекте это поле не задействовано):
```javascript
function isValidUsername(value) {
  return /^[\w.@+-]{1,150}$/u.test(value);
}

console.log(isValidUsername("user.name")); // true
console.log(isValidUsername("имя"));       // true (Unicode)
console.log(isValidUsername("bad space")); // false
```

### Валидация паролей (пароль должен состоять из 4-50 символов, содержать заглавную и строчную буквы, специальный символ, цифру, например):

```javascript
function isValidPassword(value) {
  const re = /^(?=.*[A-ZА-Я])(?=.*[a-zа-я])(?=.*\d)(?=.*[!@#\.,\/\$%\^&\*\(\)\\\-\_=\[\]{}:;"'<>\?]).*$/;
  return re.test(value);
}

console.log(isValidPassword("abc1")); // true
console.log(isValidPassword("1234")); // false (нет буквы)
console.log(isValidPassword("ab"));   // false (меньше 4 символов)
```
