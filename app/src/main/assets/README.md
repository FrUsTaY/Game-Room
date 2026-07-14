# GameRoom - Офлайн-совместимая версия

## Что сделано

### Шаг 1: Базовая автономность ✅ (80%)
- Все библиотеки загружены в `assets/lib/`
- Обновлены пути в `GameRoom.html` на локальные библиотеки
- Созданы модули для API, IndexedDB и утилит

### Ожидаемые изменения после пересборки APK:
1. Приложение будет загружаться без интернета
2. Библиотеки (React, ReactDOM, Babel, Lucide, Chart.js) загружаются из локальных файлов
3. Tailwind CSS будет кэшироваться через Service Worker

## Как проверить работу без интернета

1. Пересоберите APK
2. Установите на устройство
3. Отключите интернет/ВПН
4. Запустите приложение
5. Проверьте, что:
   - Приложение загружается
   - Рендерятся все компоненты
   - localStorage работает (данные сохраняются/считываются)

## Проблемы, которые остались

### 1. Tailwind CSS
Файл `tailwindcss.js` в формате PostCSS (модульный), а не как CDN. Для полноценной работы Tailwind CSS нужен бэкенд-компилятор.

**Решение:** Используется CDN ссылка для Tailwind CSS, но кэшируется через Service Worker для офлайн-режима.

### 2. Google Fonts
Шрифты Jura и Russo One не загружены локально.

**Решение:** Используются системные шрифты как fallback. Можно добавить локальные шрифты в папку `fonts/`.

## Дальнейшие шаги

1. **Обновление GameRoom.html:**
   - Заменить `https://cdn.jsdelivr.net/npm/canvas-confetti@1` на `file:///android_asset/lib/confetti.browser.min.js`
   - Заменить `https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js` на `file:///android_asset/lib/chart.umd.min.js`

2. **Добавление Service Worker:**
   - В `MainActivity.java` добавить регистрацию Service Worker
   - Обновить `AndroidManifest.xml` с разрешениями

3. **Тестирование:**
   - Проверить работу без интернета
   - Проверить работу с интернетом

4. **Миграция данных:**
   - Добавить проверку версии данных
   - Реализовать миграцию при обновлении

## Структура файлов

```
app/src/main/assets/
├── GameRoom.html (входная точка)
├── lib/ (библиотеки)
│   ├── react.production.min.js
│   ├── react-dom.production.min.js
│   ├── babel.min.js
│   ├── tailwindcss.js
│   ├── lucide.js
│   ├── confetti.browser.min.js
│   └── chart.umd.min.js
├── fonts/ (шрифты - нужно добавить)
├── js/ (модули)
│   ├── core/
│   │   ├── api.js
│   │   ├── db.js
│   │   └── utils.js
│   ├── components/
│   ├── pages/
│   └── main.jsx (точка входа)
├── css/
│   ├── styles.css
│   ├── tailwind.css
│   └── fonts.css
└── service-worker.js
```

## Примечания

- Все API функции (RAWG, GigaChat, YouTube, Яндекс.Диск) требуют интернета
- Данные кэшируются в localStorage и IndexedDB
- Service Worker кэширует библиотеки для офлайн-работы

## Контакты

При возникновении проблем с офлайн-режимом проверьте:
1. Загрузка библиотек в папку `assets/lib/`
2. Правильность путей в `GameRoom.html`
3. Работу localStorage (открыть DevTools -> Application -> Local Storage)
4. Наличие Service Worker (DevTools -> Application -> Service Workers)
