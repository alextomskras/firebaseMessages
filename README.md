# firebaseMessages

Проект для работы с push-уведомлениями через Firebase Cloud Messaging (FCM) в Android-приложениях.

## Описание

Репозиторий содержит Android-проект (сборка на Gradle), интегрированный с Firebase Messaging: приём, обработка и отображение push-уведомлений.

## Требования

- Android Studio (или другой совместимый IDE)
- JDK 8 или новее
- Android SDK
- Аккаунт Google и проект в [Firebase Console](https://console.firebase.google.com/)

## Установка

1. Клонируйте репозиторий:
   ```bash
   git clone <URL_репозитория>
   cd firebaseMessages
   ```
2. Откройте проект в Android Studio.
3. Получите файл `google-services.json` из Firebase Console и поместите его в модуль приложения (`app/`). Файл не должен попадать в репозиторий — он добавлен в `.gitignore`.
4. Выполните синхронизацию Gradle-проекта.

## Сборка и запуск

```bash
./gradlew assembleDebug   # сборка debug APK
./gradlew installDebug    # установка на подключённое устройство
```

## Структура проекта

- `app/` — основной модуль приложения
- `build.gradle` / `settings.gradle` — конфигурация сборки Gradle
- `.gitignore` — исключения Git (сгенерированные файлы, `google-services.json` и т.д.)

## Конфигурация

Настройки Firebase задаются через `google-services.json`. Идентификаторы устройств (token) для отправки тестовых сообщений можно получить из логов приложения:

```bash
adb logcat | grep -i "FirebaseInstanceId"
```

Тестовую отправку уведомления можно выполнить через Firebase Console → Cloud Messaging.

## Лицензия

Укажите лицензию проекта (по умолчанию — все права защищены).
