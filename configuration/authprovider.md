# AuthProvider

## Accept

Принимает любые пары логин-пароль

```javascript
"auth": [
  {
    "provider": {
      "type": "accept"
    }
  }
]
```

## Reject

Отклоняет любые пары логин-пароль

```javascript
"auth": [
  {
    "provider": {
      "type": "reject",
      "message": "Ведутся технические работы, приходите позже" // сообщение при авторизации
    }
  }
]
```

## MySQL

Для проверки логина и пароля лаунчсервер обращается к базе данных MySQL. 

**Этот способ НЕ подходит для сайтов с нестандартными алгоритмами хеширования.** 

**В базе данных создайте поле permissions типа BIGINT \(значение по умолчанию - 0\)**

```javascript
"auth": [
  {
    "provider": {
      "type": "mysql",
      "mySQLHolder": {
        "address": "localhost",               // адрес MySQL сервера
        "port": 3306,                         // порт MySQL сервера
        "username": "launchserver",           // имя пользователя
        "password": "password",               // пароль пользователя
        "database": "db?serverTimezone=UTC",  // база данных и установка серверной таймзоны
        "timezone": "UTC"                     // установка клиентской таймзоны
      },
      "query": "SELECT login, permission FROM users WHERE login=? AND password=MD5(?) LIMIT 1", // sql запрос
      "queryParams": [ "%login%", "%password%" ],                                               // параметры sql запроса
      "usePermission": true,
      "message": "Пароль неверный!"           // сообщение при неправильном пароле
    }
  }
 ]
```

## PostgreSQL

Для проверки логина и пароля лаунчсервер обращается к базе данных PostgreSQL.

**Этот способ НЕ подходит для сайтов с нестандартными алгоритмами хеширования.**

**В базе данных создайте поле permissions типа BIGINT \(значение по умолчанию - 0\)**

```javascript
"auth": [
  {
    "provider": {
      "type": "postgresql",
      "postgreSQLHolder": {
        "address": "localhost",               // адрес PostgreSQL сервера
        "port": 3306,                         // порт PostgreSQL сервера
        "username": "launchserver",           // имя пользователя
        "password": "password",               // пароль пользователя
        "database": "db?serverTimezone=UTC",  // база данных и установка серверной таймзоны
        "timezone": "UTC"                     // установка клиентской таймзоны
      },
      "query": "SELECT login, permission FROM users WHERE login=? AND password=MD5(?) LIMIT 1", // sql запрос
      "queryParams": [ "%login%", "%password%" ],                                               // параметры sql запроса
      "usePermission": true,
      "message": "Пароль неверный!"           // сообщение при неправильном пароле
    }
  }
 ]
```

## Request

Для проверки логина и пароля лаунчсервер обращается к сайту по протоколу HTTP/HTTPS 

**Ответ сервера должен выглядеть так: OK:Gravit:0, где Gravit - ваш никнейм, 0 - маска permissions**

```javascript
"auth": [
  {
    "provider": {
      "type": "request",
      "usePermission": true,
      "url": "http://gravit.pro/auth.php?username=%login%&password=%password%&ip=%ip%",
      "response": "OK:(?<username>.+):(?<permissions>.+)"
    }
  }
]
```

## Json

Для проверки логина и пароля лаунчсервер обращается к сайту по протоколу HTTP/HTTPS, но в отличии от request делает POST запрос с json данными внутри

```javascript
"auth": [
  {
    "provider": {
      "type": "json",
      "url": "http://gravit.pro/auth.php", // ссылка до скрипта проверки логина-пароля
      "apiKey": "none"                     // секретный ключ, который может проверятся в скрипте, для безопасности
    }
  }
]
```

Запрос:

```javascript
{
  "username": "admin",
  "password": "password",
  "ip": "127.0.0.1",
  "apiKey": "none"
}
```

Ответ:

```javascript
{
  "username": "admin",
  "permissions": 0
}
```

Ошибка:

```javascript
{
  "error": "Неверный логин или пароль"
}
```

## Hibernate

Hibernate — самая популярная реализация спецификации JPA, предназначенная для решения задач объектно-реляционного отображения \(ORM\) Для проверки логина и пароля лаунчсервер обращается к любой базе данных 

**Для подключения к базам данных, в libraries необходимо положить библиотеку для поддержки соответствующей базы данных** 

[Инструкция по настройке Hibernate](hibernate.md)

```javascript
"auth": [
  {
    "provider": {
      "type": "hibernate"
    }
  }
]
```

