# Lotus — GitHub Pages + GitHub Actions

Здесь не нужен постоянно работающий Node.js-сервер.

## Структура

```text
/
├─ index.html
├─ rates.json
└─ .github/
   └─ workflows/
      └─ update-rates.yml
```

## Схема

```text
Банк России
     ↓
GitHub Actions
     ↓
rates.json
     ↓
GitHub Pages
     ↓
Telegram Mini App
```

Официальный XML ЦБ РФ используется как источник ежедневных курсов USD и EUR.

## Публикация

1. Создай новый GitHub repository.
2. Загрузи туда все файлы из архива вместе с папкой `.github`.
3. Открой `Settings → Pages`.
4. В `Build and deployment` выбери `Deploy from a branch`.
5. Выбери `main` и `/ (root)`, нажми `Save`.

GitHub даст адрес вида:

`https://USERNAME.github.io/REPOSITORY/`

Этот URL потом используется как URL Telegram Mini App.

## Первый запуск

После загрузки репозитория:

`Actions → Update currency rates → Run workflow`

После успешного запуска `rates.json` заполнится.

## Автоматическое обновление

Workflow:
- запускается вручную;
- запускается примерно в 00:05 по Москве;
- дополнительно проверяет ЦБ каждый час.

Это лучше, чем пытаться создать новый курс в браузере ровно в 00:00: новый дневной курс ЦБ может стать доступен позже полуночи.

GitHub Actions поддерживает IANA timezone в `on.schedule`; GitHub также предупреждает, что scheduled jobs могут задерживаться при высокой нагрузке.

## Почему сайт не зависнет

Mini App не делает запрос к `cbr.ru`.

Он просто читает:

`./rates.json`

Поэтому CORS ЦБ больше не мешает.

Если интернет или GitHub временно не отвечает, приложение покажет последний сохранённый курс из `localStorage`, если он уже был загружен.

## Дальше

В эту же систему можно добавить второй JSON с курсами туроператоров и отдельный workflow/скрипт, который будет собирать их сайты.
