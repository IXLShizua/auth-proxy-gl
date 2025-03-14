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
      "token": "eyJhbGciOiJFUzI1NiJ9...",
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
}
```

#### **Описание параметров**

- **`api`** - настройки прокси:
    - `host` - IP-адрес, на котором будет работать прокси (например, `127.0.0.1` для локального запуска).
    - `port` - порт для подключения (например, `10000`).
- **`meta`** - мета-информация:
    - `public` - публичный URL прокси.
- **`servers`** - список поддерживаемых серверов:
    - `<ServerName>` - название сервера (например, `MyMinecraftServer`).
    - `api` - WebSocket URL лаунч-сервера.
    - `token` - токен для проверки подлинности.
    - `meta.assets` - ресурсы сервера:
        - `skins` - ссылки на скины.
        - `capes` - ссылки на плащи.
    - `experimental` - экспериментальные возможности:
        - `rewrite` - настройка перезаписи адресов ресурсов на адреса прокси:
            - `skins` - включение/отключение перезаписи ссылок на скины.
            - `capes` - включение/отключение перезаписи ссылок на плащи.

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