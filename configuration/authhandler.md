# AuthHandler

## Memory

UUID получается путем преобразования бинарного представления ника Каждому нику будет соответствовать ровно один UUID

```javascript
"auth": [
  "handler": {
    "type": "memory"
  }
]
```

## MySQL

Для получения UUID лаунчсервер обращается к базе данных MySQL

```javascript
"auth": [
  "handler": {
    "type": "mysql",
    "mySQLHolder": {
      "address": "localhost",              // адрес mysql сервера
      "port": 3306,                        // порт mysql сервера
      "username": "launchserver",          // имя пользователя
      "password": "password",              // пароль пользователя
      "database": "db?serverTimezone=UTC", // база данных (до ?), после находится установка серверной таймзоны
      "timezone": "UTC"                    // установка клиентской таймзоны
    },
    "table": "users",                      // таблица
    "uuidColumn": "uuid",                  // название столбца с uuid
    "usernameColumn": "username",          // название столбца с именами пользователей
    "accessTokenColumn": "accessToken",    // название столбца с accessToken
    "serverIDColumn": "serverID"           // название столбца с serverID
  }
]
```

## PostgreSQL

Для получения UUID лаунчсервер обращается к базе данных PostgreSQL

```javascript
"auth": [
  "handler": {
    "type": "postgresql",
    "postgreSQLHolder": {
      "address": "localhost",              // адрес postgresql сервера
      "port": 3306,                        // порт postgresql сервера
      "username": "launchserver",          // имя пользователя
      "password": "password",              // пароль пользователя
      "database": "db?serverTimezone=UTC", // база данных (до ?), после находится установка серверной таймзоны
      "timezone": "UTC"                    // установка клиентской таймзоны
    },
    "table": "users",                      // таблица
    "uuidColumn": "uuid",                  // название столбца с uuid
    "usernameColumn": "username",          // название столбца с именами пользователей
    "accessTokenColumn": "accessToken",    // название столбца с accessToken
    "serverIDColumn": "serverID",          // название столбца с serverID
    "queryByUUIDSQL": "SELECT uuid, username, NULLIF(\"accessToken\", '') as \"accessToken\", NULLIF(\"serverID\", '') as \"serverID\" FROM users WHERE uuid=? LIMIT 1",
    "queryByUsernameSQL": "SELECT uuid, username, NULLIF(\"accessToken\", '') as \"accessToken\", NULLIF(\"serverID\", '') as \"serverID\" FROM users WHERE username=? LIMIT 1",
    "updateAuthSQL": "UPDATE users SET username=?, \"accessToken\"=?, \"serverID\"=null WHERE uuid=?",
    "updateServerIDSQL": "UPDATE users SET \"serverID\"=? WHERE uuid=?"
  }
]
```

## Json

## Hibernate

Hibernate — самая популярная реализация спецификации JPA, предназначенная для решения задач объектно-реляционного отображения \(ORM\) Для проверки логина и пароля лаунчсервер обращается к любой базе данных 

**Для подключения к базам данных, в libraries необходимо положить библиотеку для поддержки соответствующей базы данных** 

[Инструкция по настройке Hibernate](hibernate.md)

```javascript
"auth": [
  {
    "handler": {
      "type": "hibernate"
    }
  }
]
```

