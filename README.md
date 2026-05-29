# SteamOS Mini PC — домашний центр развлечений

## Цели проекта

Собрать компактную игровую и мультимедийную приставку для ТВ на базе mini PC Ryzen H255 + 24 GB RAM + 500GB SSD.

Основные требования:

* console-like UX
* управление полностью с геймпада
* минимум обслуживания
* быстрый suspend/resume
* запуск сразу в Steam UI
* поддержка игр, эмуляторов и стриминга
* просмотр:
  * YouTube
  * Кинопоиск
  * VK Video
  * DLNA
  * локальной медиатеки
* интеграция с NAS
* отсутствие desktop-oriented UX

---

## Базовая архитектура

### SteamOS Mini PC

Роль:

* игровая приставка
* media client
* streaming client
* emulator console

На устройстве НЕ размещаются:

* docker stack
* VM
* NAS storage
* AI workloads
* torrent stack

Основной принцип:

* appliance-first
* минимум фоновых сервисов
* максимальная стабильность suspend/resume

### NAS

Роль:

* хранение медиа
* Jellyfin server (если нужно)
* torrent-качалка
* ROM storage
* metadata
* backups

### Virtualization Host

Роль:

* Docker
* VMs
* HomeLab
* AI
* CI/CD
* сервисы инфраструктуры

---

## Базовая ОС

### ОС

* SteamOS

Причины выбора:

* понятная архитектура
* опыт эксплуатации
* console UX
* gamescope integration
* хорошая поддержка AMD GPU

Допускается попробовать Nobara

---

## Основной UX

### Boot flow

```text
Power On
  ↓
SteamOS
  ↓
Gamescope Session
  ↓
Steam Big Picture
```

Desktop mode используется только для:

* обновлений
* настройки
* диагностики

---

## Основной software stack

### Игры

#### Steam

Роль:

* основная игровая библиотека
* Big Picture UI
* couch gaming

### Emulation

#### EmulationStation

Роль:

* единый frontend для эмуляторов

### RetroDeck

Роль:

* ретро-консоли

---

## Media stack

### Kodi

Роль:

* media center
* fallback media UI
* локальное видео
* plugins

План:

* запуск как non-steam app
* управление полностью с gamepad

### Jellyfin

#### Сервер

Размещается:

* NAS

#### Клиент

Размещается:

* SteamOS mini PC

### DLNA

#### Сервер

Размещается:

* NAS

#### Клиент

Размещается:

* SteamOS mini PC

Варианты:

* Kodi plugin
* Synology Video
* ABXY Video

---

## Streaming stack

### Sunshine

Размещение:

* SteamOS mini PC
* optionally gaming PC

Роль:

* game streaming host

### Moonlight

Размещение:

* SteamOS mini PC

Роль:

* streaming client

Использование:

* heavy AAA gaming
* remote gaming
* streaming from main PC

---

## Browser stack

### Основной browser

Планируется:

* Chromium
  или
* Firefox
  или
* YandexBrowser for Android TV

Требования:

* kiosk/fullscreen support
* gamepad navigation
* hardware video decode

---

## Кинопоиск

### Цель

Полноценная работа:

* с геймпада
* fullscreen
* без мышки

### Варианты интеграции

#### Вариант 1 — Browser App (приоритетный)

Отдельный fullscreen launcher:

```text
Steam → Non-Steam App → Kinopoisk
```

Запуск:

* kiosk mode
* fullscreen
* отдельный профиль браузера

Плюсы:

* проще
* стабильнее
* поддержка DRM

Минусы:

* browser UX

---

### Управление геймпадом

План:

* Steam Input mapping
* right stick → mouse
* triggers → scroll
* A/B → Enter/Escape

Дополнительно:

* CSS scaling для TV
* oversized UI

---

## VK Video

### Цель

Просмотр:

* fullscreen
* gamepad-first
* couch UX

### Варианты интеграции

#### Browser App

Отдельный launcher через Steam.

Плюсы:

* нативная поддержка сайта
* меньше проблем с авторизацией

Минусы:

* web UX

---

## YouTube

### Варианты

#### Browser App

или

#### Kodi plugin

---

## Controller UX

### Основной принцип

Система должна быть usable:

* без клавиатуры
* без мыши

### Steam Input Profiles

Планируется отдельный профиль:

#### Media Profile

Mapping:

* Left Stick → navigation
* Right Stick → mouse
* RT/LT → scroll
* A → Enter
* B → Escape
* X → Context
* Y → Search
* Start → Fullscreen
* Select → Back

---

## Gamescope

### План

Использовать gamescope-session как основной shell.

### Tuning goals

* fixed refresh
* stable frametime
* FSR scaling
* suspend stability
* HDR experimentation
* couch-friendly performance profiles

---

## Производительность

### Основной режим

Цель:

* тихая работа
* низкий шум
* минимальный fan spin

### TDP Profiles

#### Silent

15W

Использование:

* media
* retro
* indie

#### Balanced

25W

Использование:

* большинство игр

#### Performance

35W

Использование:

* AAA
* эмуляция PS3/Switch

---

## Сетевые сервисы

### Syncthing

Роль:

* sync saves
* ROM sync
* configs
* screenshots

### (?) Tailscale

Роль:

* remote access
* remote streaming
* maintenance

---

## Будущие задачи

* HDMI-CEC
* wake from controller
* HDR validation
* VRR testing
* suspend reliability
* автоматическое переключение TDP
* единый media launcher
* child-friendly profiles
* parental control
* unified remote-friendly UI

---

## Дополнение — Voice Control & Voice Search

### Цели

Система должна поддерживать:

* голосовой поиск
* голосовое управление
* media navigation
* запуск приложений голосом
* поиск фильмов/видео
* поиск игр

---

## Приоритетные сценарии

### Media Search

Примеры:

* "включи Интерстеллар"
* "найди мультики"
* "включи VK Video"
* "поставь YouTube"

### System Control

Примеры:

* "выключи приставку"
* "открой Steam"
* "запусти эмулятор"

### Game Launching

Примеры:

* "запусти Cyberpunk"
* "открой Minecraft"

---

## Возможные технологии

### Speech-to-Text

Кандидаты:

* Whisper
* faster-whisper
* Vosk

### Voice Assistant Layer

Кандидаты:

* Home Assistant Assist
* custom local assistant
* LLM-powered assistant

### Audio Input

Варианты:

* USB microphone
* gamepad headset
* Bluetooth microphone
* remote control microphone

## Требования

* local-first architecture
* low latency
* работа без облака
* gamepad-compatible UX
* overlay interface
* wake-word support (optional)

## Возможные UX варианты

### Push-to-talk

Через:

* кнопка на геймпаде
* кнопка на пульте

### Overlay UI

Полупрозрачный overlay:

* поверх gamescope
* поверх Steam
* поверх browser apps

---

## Будущие исследования

* overlay implementation
* gamescope integration
* microphone routing
* wake word
* speech recognition quality
* language models
* TV-friendly voice UX

