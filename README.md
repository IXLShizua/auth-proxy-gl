# Auth-Proxy-GL

**Прокси для взаимодействия между [Authlib Injector](https://github.com/yushijinhun/authlib-injector/)
и [Gravit Launcher](https://gravitlauncher.com/)**

---

## Установка и настройка

### 1. Скачивание

Перейдите на страницу [релизов GitHub](https://github.com/IXLShizua/auth-proxy-gl/releases/latest) и загрузите последнюю
версию прокси, подходящую для вашей операционной системы.

### 2. Первый запуск

Запустите прокси, чтобы автоматически создать файл конфигурации `config.json`.

### 3. Настройка `config.json`

Откройте созданный файл `config.json`, он будет выглядеть следующим образом:

```json
{
  "api": {
    "host": "127.0.0.1",
    "port": 10000
  },
  "meta": {
    "public": "http://127.0.0.1:10000"
  },
  "servers": {
    "MyMinecraftServer": {
      "api": "wss://launcher.example.com/api",
      "token": "eyJhbGciOiJFUzI1NiJ9..."
    }
  }
}
```

В конечном итоге Вы будете должны прийти к примерно такому:

```json
{
  "api": {
    "host": "127.0.0.1",
    "port": 10000
  },
  "meta": {
    "public": "http://127.0.0.1:10000"
  },
  "servers": {
    "MyMinecraftServer": {
      "api": "wss://launcher.example.com/api",
      "token": "eyJhbGciOiJFUzI1NiJ9..."
    },
    "meta": {
      "assets": {
        "skins": [
          "skins.example.com"
        ],
        "capes": [
          "capes.example.com"
        ]
      }
    },
    "experimental": {
      "rewrite": {
        "skins": false,
        "capes": false
      }
    }
  }
}
```

### **Описание параметров**

### `api` – Настройки прокси-сервера

- **`host`** _(обязательный)_  
  IP-адрес, на котором будет работать прокси. Для локального запуска используйте `127.0.0.1`.

- **`port`** _(обязательный)_  
  Порт, на котором будет слушать прокси.

### `meta` – Мета-информация

- **`public`** _(обязательный)_  
  Публичный URL, по которому доступен прокси.

### `servers` – Список серверов

Каждый ключ — это имя сервера (например, `MyMinecraftServer`) с такими параметрами:

- **`api`** _(обязательный)_  
  WebSocket URL лаунч-сервера.

- **`token`** _(обязательный)_  
  Токен, используемый для аутентификации сервера.

- **`meta.assets`** _(обязательный)_  
  Ссылки на ресурсы сервера:
    - **Вариант 1: Объединённый формат**
      ```json
      "assets": [
        "resources.example.com"
      ]
      ```
    - **Вариант 2: Разделённый формат**
      ```json
      "assets": {
        "skins": ["skins.example.com"],
        "capes": ["capes.example.com"]
      }
      ```

- **`experimental`** _(необязательный)_  
  Экспериментальные функции:
    - **`rewrite`** _(необязательный)_ – Управляет перезаписью URL для запросов к ресурсам.
        - **Вариант 1: Объединённый формат**
          ```json
          "rewrite": true
          ```
        - **Вариант 2: Разделённый формат**
          ```json
          "rewrite": {
            "skins": true,
            "capes": false
          }
          ```

## Настройка Authlib-Injector

### Что такое Authlib-Injector?

**Authlib-Injector** заменяет официальную систему авторизации Minecraft на кастомную, позволяя использовать сторонние
лаунчеры.

### Установка

1. Скачайте последнюю версию [Authlib-Injector](https://github.com/yushijinhun/authlib-injector/releases/latest).
2. Переместите `authlib-injector.jar` в папку с сервером.

### Настройка запуска сервера

Используйте следующую команду для запуска Minecraft-сервера с Authlib-Injector и Auth-Proxy-GL:

```bash
java -javaagent:authlib-injector.jar=http://127.0.0.1:10000/MyMinecraftServer -jar server.jar
```

#### **Разбор команды:**

- `-javaagent:authlib-injector.jar` — путь к файлу **Authlib-Injector**.
- `http://127.0.0.1:10000/MyMinecraftServer` — адрес вашей прокси:
    - `127.0.0.1` — IP-адрес прокси.
    - `10000` — порт, указанный в `config.json`.
    - `MyMinecraftServer` — название сервера из `config.json`.
- `-jar server.jar` — запуск основного сервера Minecraft.

### **Дополнительные рекомендации**

- Если используете HTTPS, настройте SSL-сертификат.
- Убедитесь, что порт прокси совпадает с указанным в Authlib-Injector.

Теперь ваш сервер полностью готов к работе с **Authlib-Injector** и **Auth-Proxy-GL**! 🎮