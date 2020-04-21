# Hibernate

Hibernate — самая популярная реализация спецификации JPA, предназначенная для решения задач объектно-реляционного отображения \(ORM\) Для работы, лаунчсервер подключается к любой базе данных, будь то SQLite, MSSQL или другие, у которых есть библиотеки под java. 

**Библиотеки \(драйвера\) необходимо скачать из интернета, и положить в папку libraries.** **Эта секция должна быть в LaunchServer.json, прям в корне, до последней }**

```javascript
    "stripLineNumbers": true,
    "deleteTempFiles": true,
    "startScript": "./start.sh",                            // тут нужна ,
    "dao": {
        "type": "hibernate",
        "driver": "org.postgresql.Driver",                  // класс драйвера
        "url": "jdbc:postgresql://localhost/launchserver",  // URL базы данных
        "username": "launchserver",                         // имя пользователя
        "password": "xxxxx",                                // пароль
        "pool_size": "4"                                    // сколько параллельных запросов к базе данных можно выполнять?
    }
}
```

Иногда Hibernate просит указать SQL диалект для некоторых драйверов. Список стандартных диалектов можно посмотреть тут. Некоторый неполный список драйверов, с которыми работает Hibernate:

```text
org.hsqldb.jdbcDriver(URL jdbc:hsqldb:hsql://localhost, Dialect org.hibernate.dialect.HSQLDialect) - HSQL - встроеная база данных в оперативной памяти. Только для тестирования
org.postgresql.Driver(URL jdbc:postgresql://localhost/launchserver, Dialect org.hibernate.dialect.PostgreSQLDialect) - PostgreSQL - конфигурация по умолчанию
com.mysql.jdbc.Driver (URL jdbc:mysql://localhost/launchserver, Dialect org.hibernate.dialect.MySQLDialect) - MySQL (MariaDB/PersonaServer)
```

## Автосоздание таблиц

У Hibernate есть возможность автоматически создать необходимые таблицы для своей работы для любой поддерживаемой БД. После настройки параметров подключения запустите лаунчсервер с опцией JVM `-Dhibernate.hbm2ddl.auto=ЗНАЧЕНИЕ`, где выберите одно из следующих значений:

```text
validate - только проверка, никаких изменений в базу не будет внесено
update - обновление структуры таблиц без потери данных
create - удаление всех данных из БД и повторное создание таблиц
```

