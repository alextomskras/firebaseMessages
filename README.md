# firebaseMessages

Проект для работы с push-уведомлениями через Firebase Cloud Messaging (FCM) в Android-приложениях.

> **Важно:** на данный момент в репозитории отсутствуют исходники — только `.gitignore` и `README.md`.
> Документация ниже описывает план/требования к проекту; разделы про SDK и миграцию будут уточнены,
> когда код будет добавлен в репозиторий.

## Описание (планируемое)

Android-приложение (сборка на Gradle), интегрированное с Firebase:
- приём, обработка и отображение push-уведомлений (FCM);
- хранение картинок (изображений сообщений) в **Firebase Storage (buckets)**;
- сохранение сообщений в базу данных **вместе с ссылками на изображения** (предполагаемая архитектура — проверить по коду после его добавления).

## Требования

- Android Studio (или другой совместимый IDE)
- JDK 8 или новее (для AGP 8.x — JDK 17)
- **Android SDK API 34 (Android 14)** — целевая версия SDK проекта; устанавливать через SDK Manager
- Аккаунт Google и проект в [Firebase Console](https://console.firebase.google.com/) с включёнными FCM и Storage

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
adb logcat | grep -i "FirebaseMessaging"
```

Тестовую отправку уведомления можно выполнить через Firebase Console → Cloud Messaging.

## Миграция на Android SDK 34 (API 34, Android 14)

При переходе проекта на compileSdk/targetSdk 34 обычно требуются следующие изменения (применимы, когда появится код):

1. **build.gradle (app):**
   ```groovy
   android {
       compileSdk 34
       defaultConfig {
           targetSdk 34
           minSdk 21 // или выше
       }
   }
   ```
2. **Gradle / AGP:** AGP ≥ 8.1 и Gradle ≥ 8.0 (для AGP 8.x обязателен JDK 17).
3. **Android 14 (API 34) — обязательные изменения:**
   - `PendingIntent` должен явно указывать `FLAG_IMMUTABLE` или `FLAG_MUTABLE` (для push/уведомлений обычно `FLAG_IMMUTABLE`);
   - foreground-сервисы обязаны декларировать `android:foregroundServiceType` и соответствующие разрешения (`FOREGROUND_SERVICE_*`); при запуске FCM-сервиса из фона — ограничения на background start;
   - точные алармы: `SCHEDULE_EXACT_ALARM` требует обоснования, лучше `USE_EXACT_ALARM` только для будильников/календарей;
   - обновить зависимости AndroidX до версий, скомпилированных против API 34.
4. **Firebase SDK:** актуальные версии `firebase-messaging` и `firebase-storage` (BOM `com.google.firebase:firebase-bom`).
5. Установить сам SDK: Android Studio → SDK Manager → SDK Platforms → Android 14 (API 34), либо headless:
   ```bash
   sdkmanager "platforms;android-34" "build-tools;34.0.0"
   ```

## Лицензия

Укажите лицензию проекта (по умолчанию — все права защищены).
