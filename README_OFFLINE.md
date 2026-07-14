# GameRoom - Офлайн Руководство

## Статус
- ✅ Phase 1: Базовая автономность (100%)
- ✅ Phase 2: Интеграция модулей (100%)
- ✅ Phase 3: Офлайн режим (100%)
- ✅ Phase 4: Система кэширования (100%)
- ✅ Phase 5: Кэширование данных (100%)
- ⏳ Phase 6: Тестирование (в процессе)

## Что работает офлайн

### ✅ Базовый функционал
- Загрузка приложения без интернета
- Отображение уже добавленных игр
- Просмотр вишлиста
- Просмотр списка пройденных игр
- Добавление игр вручную (без RAWG)
- Редактирование игр
- Удаление игр
- Настройки приложения

### ✅ Кэширование данных
- API ответы кэшируются на 1 час
- Обложки игр кэшируются как blobs
- Данные сохраняются в localStorage

## Как протестировать офлайн режим

### 1. Отключить интернет
- Отключить Wi-Fi на устройстве
- Или включить режим "В самолете"

### 2. Запустить приложение
- Открыть GameRoom
- Приложение должно загрузиться без ошибок
- В хедере показывается режим: `OFFLINE`

### 3. Проверить функции
- ✅ Список игр должен отображаться
- ✅ Вишлист должен отображаться
- ✅ Список пройденных игр должен отображаться
- ❌ Поиск игр через RAWG отключен
- ❌ AI инсайты через GigaChat отключены
- ❌ Трейлеры YouTube отключены

### 4. Включить интернет
- Вернуть Wi-Fi
- Режим должен переключиться на `ONLINE` или `PARTIAL`
- Данные синхронизируются

## Таблица режимов работы

| Режим | Базовые функции | API функции | Синхронизация |
|-------|----------------|-------------|---------------|
| ONLINE | ✅ | ✅ | ✅ |
| OFFLINE | ✅ | ❌ | ❌ |
| PARTIAL | ✅ | ⚠️ (кэш) | ⚠️ (очередь) |

## Визуальные индикаторы

### Режим в хедере:
- **ONLINE:** 🌐
- **PARTIAL:** ⚠️
- **OFFLINE:** 📡

### Отключенные кнопки:
- Поиск RAWG: "Требуется интернет"
- AI инсайты: "Требуется интернет"
- Трейлеры: "Требуется интернет"

## Архитектура кэширования

### Level 1: LocalStorage (малые данные)
```
├── games
├── wishlist
├── playedList
├── settings
├── ai_messages
└── api_cache_* (1 час TTL)
```

### Level 2: IndexedDB (большие данные)
```
├── game_covers (blobs)
└── api_cache (1 час TTL)
```

### Level 3: Cache API (Service Worker)
```
├── lib/*.js (React, ReactDOM, Babel, Tailwind, Lucide, Chart.js)
├── css/*.css
└── fonts/* (Jura, Russo One)
```

## Файлы приложения

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
├── fonts/ (шрифты)
│   ├── Jura/
│   │   ├── Jura-Regular.ttf
│   │   ├── Jura-Medium.ttf
│   │   └── Jura-Bold.ttf
│   └── RussoOne/
│       └── RussoOne-Regular.ttf
├── js/
│   ├── core/
│   │   ├── state.js
│   │   ├── utils.js
│   │   └── api.js
│   ├── components/
│   │   ├── GameCard.jsx
│   │   ├── Icon.jsx
│   │   └── CustomSelect.jsx
│   ├── pages/
│   │   ├── GamesPage.jsx
│   │   ├── WishlistPage.jsx
│   │   └── SettingsPage.jsx
│   └── main.jsx
├── css/
│   ├── styles.css
│   └── fonts.css
└── data/
    └── hltb_cookies.txt
```

## Функции IndexedDB

### Кэширование API ответов:
```javascript
// Сохранить в кэш
window.db.saveAPICache(key, data, ttl);

// Получить из кэша
const cached = await window.db.getAPICache(key);
```

### Кэширование обложек:
```javascript
// Сохранить обложку как blob
window.db.saveGameCover(url, blob);

// Получить обложку
const blob = await window.db.getGameCover(url);
```

## Технические детали

### TTL кэша:
- API ответы: 1 час (3600000 мс)
- Кэш в localStorage: 1 час (3600000 мс)

### Определение режима:
```javascript
const mode = detectMode();
// ONLINE, OFFLINE, или PARTIAL
```

### Обработка ошибок API:
```javascript
const result = await searchRAWG(search, apiKey);
if (result.error === 'OFFLINE') {
    // Показать сообщение об офлайн режиме
}
```

## Отладка

### Проверить кэш:
```javascript
// В консоли браузера (если включить debug mode)
console.log(localStorage.getItem('api_cache_rawg_search_test'));
```

### Очистить кэш:
```javascript
localStorage.clear();
// или
indexedDB.deleteDatabase('GameRoomDB');
```

## Планы улучшений

### Phase 7: Обновление данных
- Автоматическое обновление кэша при восстановлении сети
- Очередь синхронизации для изменений

### Phase 8: Обновления приложения
- Автоматическая загрузка обновлений библиотек
- Версионирование данных

### Phase 9: Экспорт данных
- Сохранение в JSON/PDF
- Резервное копирование

---

**Версия:** 2.1  
**Последнее обновление:** 2026-06-18  
**Разработчик:** GameRoom Team
