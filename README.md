# LibertyRP — загрузочный экран (loading screen)

Одностраничный экран подключения для GMod DarkRP-сервера **LibertyRP**.
Всё в одном файле **`index.html`** — шрифты (Oswald + JetBrains Mono с
кириллицей) и логотип вшиты как data-URI, внешних зависимостей нет.

## 🌐 Живой адрес

Сайт захостен на GitHub Pages:

> **https://bansheegit.github.io/libertyrp-loadingscreen/**

Пропишите его в `server.cfg` сервера:

```
sv_loadingurl "https://bansheegit.github.io/libertyrp-loadingscreen/"
```

После перезапуска сервера игрок при заходе увидит этот экран.

## Что внутри

- Оформление «ночной неоновый город», синие тона под логотип.
- **Сменяющийся фон** — генеративные сцены (полночь, дождь, снег, гроза, туман)
  с параллаксом, звёздами и погодой. Можно заменить своими картинками.
- **Музыка с автосменой треков** — плеер по кругу играет плейлист (кнопки
  вперёд/назад/пауза, эквалайзер, ползунок). Сейчас плейлист пуст — плеер в
  режиме ожидания «Радио · Скоро в эфире».
- **Прогресс подключения** — работает с настоящим GMod API
  (`GameDetails`, `SetFilesTotal`, `SetFilesNeeded`, `DownloadingFile`,
  `AllDownloadsComplete`): имена скачиваемых файлов на живом сервере — реальные.
  Вне игры крутится демо-загрузка на реальных путях контента сервера.
- Ротатор советов/правил, панель с инфо о сервере, адаптивная вёрстка.

## Настройка

Откройте `index.html`, вверху `<script>` — блок **`CONFIG`**.

### Музыка

```js
tracks:[
  {title:"Midnight Drive", artist:"Liberty FM", src:"https://cdn.libertyrp.ru/music/track1.mp3"},
  {title:"City of Freedom", artist:"Liberty FM", src:"https://cdn.libertyrp.ru/music/track2.mp3"},
],
```

Прямые ссылки на `.mp3` / `.ogg` по HTTPS (с CORS). Треки играют по кругу с
автопереключением. Если пусто — плеер в режиме ожидания.

### Свои фоны-кадры

```js
backgrounds:[
  "https://cdn.libertyrp.ru/bg/city1.jpg",
  "https://cdn.libertyrp.ru/bg/city2.jpg",
],
```

Не пусто → фон крутит эти картинки (кросс-фейд каждые 7 c). Пусто → генеративный город.

### Данные сервера

```js
server:{ name:"LIBERTY RP", address:"play.libertyrp.ru:27015",
         map:"rp_liberty_city", gamemode:"DarkRP", maxplayers:128, ping:24 }
```

При реальном подключении GMod вызывает `GameDetails(...)` и сам перезапишет имя
сервера, карту, режим и слоты.

## Обновление сайта

После правок в `index.html`:

```
git add -A
git commit -m "update loading screen"
git push
```

GitHub Pages пересоберётся автоматически за ~1 минуту.

## Про автозвук в GMod

Загрузочный экран GMod работает на Chromium (CEF). Если автозвук разрешён —
музыка стартует сама; если заблокирован, показывается плашка «Нажмите, чтобы
войти» (появляется только когда есть треки). Кнопка «Звук вкл/выкл» — сверху справа.

## Предпросмотр локально

```
npx http-server -p 8080
```

и откройте `http://127.0.0.1:8080/index.html` (протокол `file://` браузеры блокируют).
