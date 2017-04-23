# Задание 3

Мобилизация.Гифки – сервис для поиска гифок в перерывах между занятиями.

Сервис написан с использованием [bem-components](https://ru.bem.info/platform/libs/bem-components/5.0.0/).

Работа избранного в оффлайне реализована с помощью технологии [Service Worker](https://developer.mozilla.org/ru/docs/Web/API/Service_Worker_API/Using_Service_Workers).

Для поиска изображений используется [API сервиса Giphy](https://github.com/Giphy/GiphyAPI).

В браузерах, не поддерживающих сервис-воркеры, приложение так же должно корректно работать,
за исключением возможности работы в оффлайне.

## Структура проекта

  * `gifs.html` – точка входа
  * `assets` – статические файлы проекта
  * `vendor` –  статические файлы внешних библиотек
  * `service-worker.js` – скрипт сервис-воркера

Открывать `gifs.html` нужно с помощью локального веб-сервера – не как файл.
Это можно сделать с помощью встроенного в WebStorm/Idea веб-сервера, с помощью простого сервера
из состава PHP или Python. Можно воспользоваться и любым другим способом.


## Решение

- `Service-worker.js` помещен в `/assets`, поэтому область видимости будет ограничена `/assets`, для того чтобы заработало, переместим Service-worker.js в корень. И поменяем путь к сервис-воркеру в файле `blocks.js` на `./service-worker.js`.
- После добавим `cacheKey.includes('gifs.html')` к условиям в функции `needStoreForOffline()`, для загрузки страницы из кеша.
- Потом добавим файлы для кеширования перед установкой в массив `CACHE_MAIN_FILES` и используем массив новой функцией `beforeCacheFiles()`.
- Далее изменим обработку запросов на строчке 55 в `Service-worker.js`:
  ```javascript
  let response;
       if (needStoreForOffline(cacheKey)) {
              response = fetchAndPutToCache(cacheKey, event.request);
          } else {
              response = fetchWithFallbackToCache(event.request);
          }
          event.respondWith(response);
  });
  ```
  
