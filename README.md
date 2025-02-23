# Auth-Proxy-GL

**Прокси для взаимодействия между [Authlib Injector](https://github.com/yushijinhun/authlib-injector/) и [Gravit Launcher](https://gravitlauncher.com/)**

---

## Установка и настройка

1. **Скачайте прокси**  
   Перейдите на страницу [релизов GitHub](https://github.com/IXLShizua/auth-proxy-gl/releases/latest) и скачайте версию прокси, подходящую для вашей операционной системы.

2. **Первый запуск**  
   Запустите прокси, чтобы она автоматически создала файл конфигурации `config.json`.

3. **Настройте файл `config.json`**  
   Откройте созданный файл `config.json`, там Вы увидите такую структуру:

```json
{
  "api": {
    "host": "0.0.0.0",
    "port": 10000
  },
  "keys": {
    "private": "",
    "public": ""
  },
  "servers": {
    "SERVER-NAME": {
      "api": "LAUNCHSERVER-ADDRESS",
      "token": "CHECK-SERVER-TOKEN"
    }
  }
}
```

---

### **Описание секций в `config.json`**

#### **1. Секция `api`**
Настройки для работы прокси:
- `host` — IP-адрес, на котором прокси будет слушать запросы.  
  Например, для локальной работы используйте `0.0.0.0`.
- `port` — порт для подключения к прокси.  
  Например, порт `10000`.

---

#### **2. Секция `keys`**
Ключи для обеспечения безопасности между компонентами.

- `private` — приватный (закрытый) ключ.
- `public` — публичный (открытый) ключ.

**Как создать ключи?**

1. Создайте приватный ключ:
   ```bash
   openssl genrsa -out private.pem 4096
   ```
   Укажите путь до файла `private.pem` в поле `private`.

2. Создайте публичный ключ:
   ```bash
   openssl rsa -in private.pem -pubout -out public.pem
   ```
   Укажите путь до файла `public.pem` в поле `public`.

---

#### **3. Секция `servers`**
Настройки серверов, которые будут использоваться прокси.  
Каждый сервер настраивается как отдельный объект с уникальным именем.

- `SERVER-NAME` — имя вашего сервера (может быть любым, например - `1.20.1-Vanilla`).
- `api` — адрес лаунч-сервера (например - `wss://launcher.example.com/api`).
- `token` — токен сервера для проверки.

**Как получить токен?**  
Выполните команду в консоли лаунчсервера:
```bash
token server SERVER-NAME std false
```

Пример заполнения секции `servers`:
```json
"servers": {
  "1.20.1-Vanilla": {
    "api": "wss://launcher.example.com/api",
    "token": "eyJhbGciOiJFUzI1NiJ9.eyJpc3MiOiJMYXVuY2hTZXJ2ZXIiLCJzZXJ2ZXJOYW1lIjoiZjIzNmU4MGYtZGE2MC00ZjZjLTlkYTgtMWVmNmZjYzU0Zjk2IiwiYXV0aElkIjoic3RkIiwidG9rZW5UeXBlIjoiY2hlY2tTZXJ2ZXIiLCJpc1B1YmxpYyI6ZmFsc2V9.sLnTi6LcLKtjB8eg7Zqa-M84zyEXowVOYvt7zThVtG-tqA3_8vpjqzYqqDV35w4mbqBhroQ7asl0ef9Z1GBilw"
  }
}
```

---

## Настройка Authlib-Injector

### Что такое Authlib-Injector?
**Authlib-Injector** — это библиотека, заменяющий официальную систему авторизации Minecraft на кастомную, что позволяет использовать собственные лаунчеры и системы авторизации.

---

### Установка и настройка Authlib-Injector

1. **Скачайте Authlib-Injector**  
   Загрузите последнюю версию Authlib-Injector с [релизов GitHub](https://github.com/yushijinhun/authlib-injector/releases/latest).

2. **Настройте строку запуска сервера**

Для работы **Authlib-Injector** с **Auth-Proxy-GL**, используйте следующую строку запуска:

```bash
java -javaagent:authlib-injector-1.2.5.jar=http://0.0.0.0:10000/SERVER-NAME -jar server.jar
```

---

### **Объяснение компонентов строки запуска**

1. **`-javaagent:authlib-injector-1.2.5.jar=http://0.0.0.0:10000/SERVER-NAME`**
    - `authlib-injector-1.2.5.jar` — библиотека Authlib-Injector, которую необходимо скачать и поместить в папку с сервером.
    - `http://0.0.0.0:10000/SERVER-NAME` — адрес вашей прокси:
        - `0.0.0.0` — IP-адрес прокси (замените на фактический адрес, например, `127.0.0.1` или ваш публичный IP).
        - `/SERVER-NAME` — имя вашего сервера, которое вы указали в секции `servers` файла `config.json`.

2. **`-jar server.jar`**  
   Указывает, какой серверный файл запускать. Замените `server.jar` на название файла вашего сервера.

---

## **Пример для реальной настройки**

Допустим:
- Прокси расположена по адресу: `http://127.0.0.1:10000`.
- Имя сервера в конфигурации: `1.20.1-Vanilla`.

Строка запуска будет выглядеть так:

```bash
java -javaagent:authlib-injector-1.2.5.jar=http://127.0.0.1:10000/1.20.1-Vanilla -jar server.jar
```

---

### Примечания

- Убедитесь, что порт, указанный в `config.json`, совпадает с указанным в Authlib-Injector.
- Если используется HTTPS, настройте SSL-сертификат для безопасного подключения.

Теперь ваш сервер Minecraft полностью настроен для работы с **Authlib-Injector** и **Auth-Proxy-GL**! 🎮