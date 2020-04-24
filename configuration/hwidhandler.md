# HWIDHandler

## Accept

Принимает любые hwid и никуда их не записывает. Банить не получится

```javascript
"hwidHandler": {
    "type": "accept"
}
```

## Memory

Сохраняет и проверяет hwid в ОЗУ  
**При остановке лаунчсервера hwid теряются**

```javascript
"hwidHandler": {
    "type": "memory"
}
```

## JsonFile

Сохраняет hwid в файл json

```javascript
"hwidHandler": {
    "type": "jsonfile",
    "filename": "hwids.json",                 // название файла с hwid
    "banMessage": "Ваш аккаунт заблокирован!" // сообщение, когда забаненный пользователь пытается войти
}
```

## MySQL

Для проверки hwid лаунчсервер обращается к mysql  
**Для использования умного сравнения hwid необходимо поменять все "and" на "or" в запросе queryHwids**

```text
"hwidHandler": {
     "type": "mysql",

     "mySQLHolder": {
        "address": "localhost",              // адрес mysql сервера
        "port": 3306,                        // порт mysql сервера
        "username": "launchserver",          // имя пользователя
        "password": "password",              // пароль пользователя
        "database": "db?serverTimezone=UTC", // база данных (до ?), после находится установка серверной таймзоны
        "timezone": "UTC"                    // установка клиентской таймзоны
      },

     "queryHwids": "SELECT * FROM `users_hwids` WHERE `totalMemory` = ? and `serialNumber` = ? and `HWDiskSerial` = ? and `processorID` = ? and `macAddr` = ?",           // sql запрос, ? по порядку заменяются параметрами из paramsHwids
     "paramsHwids": [ "%totalMemory%", "%serialNumber%", "%HWDiskSerial%", "%processorID%", "%MAC%" ],                                                                    // параметры sql запроса

     "queryBan": "UPDATE `users_hwids` SET `isBanned` = ? WHERE `totalMemory` = ? and `serialNumber` = ? and `HWDiskSerial` = ? and `processorID` = ? and `macAddr` = ?", // sql запрос, ? по порядку заменяются параметрами из paramsBan
     "paramsBan": [ "%isBanned%", "%totalMemory%", "%serialNumber%", "%HWDiskSerial%", "%processorID%", "%MAC%" ],                                                        // параметры sql запроса

     "tableUsers": "users",                    // таблица с пользователями

     "userFieldHwid": "hwid",                  // название столбца с ID пользователя в таблице с HWIDами
     "userFieldLogin": "username",             // название столбца с именами пользователей

     "tableHwids": "users_hwids",              // таблица с hwid

     "hwidFieldBanned": "isBanned",            // название столбца с значением, забанен ли пользователь
     "hwidFieldTotalMemory": "totalMemory",    // название столбца с количеством ОЗУ пользователя (в байтах)
     "hwidFieldSerialNumber": "serialNumber",  // название столбца с серийным номером компьютера пользователя
     "hwidFieldHWDiskSerial": "HWDiskSerial",  // название столбца с серийным номером жесткого диска пользователя
     "hwidFieldProcessorID": "processorID",    // название столбца с ID процессора пользователя
     "hwidFieldMAC": "macAddr",                // название столбца с MAC адресом сетевой карты пользователя
     
     "compareMode": false,                     // умное сравнение hwid
     "compare": 50,                            // cтепень схожести hwid

     "banMessage": "Ваш аккаунт заблокирован!" // сообщение, когда забаненный пользователь пытается войти
}
```

## Json

Для проверки логина и пароля лаунчсервер обращается к сайту по протоколу HTTP/HTTPS, делает POST запрос с json данными внутри

```javascript
"hwidHandler": {
    "type": "json",
    "url": "http://gravit.pro/sethwid.php",        // ссылка до скрипта занесения hwid (в базу/etc)
    "urlBan": "http://gravit.pro/banhwid.php",     // ссылка до скрипта бана пользователя
    "urlUnBan": "http://gravit.pro/unbanhwid.php", // ссылка до скрипта разбана пользователя
    "urlGet": "http://gravit.pro/gethwid.php",     // ссылка до скрипта получения hwid
    "apiKey": "none" // секретный ключ, который может проверятся в скрипте, для безопасности
}
```

## Hibernate

Hibernate — самая популярная реализация спецификации JPA, предназначенная для решения задач объектно-реляционного отображения \(ORM\) Для проверки логина и пароля лаунчсервер обращается к любой базе данных 

**Для подключения к базам данных, в libraries необходимо положить библиотеку для поддержки соответствующей базы данных** 

[Инструкция по настройке Hibernate](hibernate.md)

```javascript
"hwidHandler": {
    "type": "hibernate",
}
```

