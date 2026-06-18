# Postman Tests — Platzi Fake API

Автоматизированные API-тесты для публичного тестового сервиса [Platzi Fake API](https://fakeapi.platzi.com) (`api.escuelajs.co/api/v1`).

Коллекция запускается через **Newman** — CLI-запускатор Postman-коллекций — без необходимости открывать Postman GUI.

---

## Что тестируется

| Запрос | Метод | Проверки |
|---|---|---|
| `/auth/login` | POST | Статус 201, тело — объект |
| `/categories` | GET | Статус 200, обязательные поля, URL изображений (`https`), валидные ISO-даты |
| `/products` | GET | Статус 200, тело — массив, описание не пустое |
| `/products/?prise=100` | GET | Статус 200, время ответа < 1500 мс, тело — массив |
| `/products/?prise=25` | GET | Статус 200, время ответа < 1000 мс, тело — массив |
| `/products/?price_min=10&price_max=50` | GET | Цены всех товаров в диапазоне 10–50, статус 200, тело — массив объектов |
| `/products/?categoryId=1` | GET | Все товары принадлежат категории с id=1 |
| `/products/?title=Classic` | GET | В ответе присутствует товар со slug `classic-white-crew-neck-t-shirt` |
| `/products/?categorySlug=miscellaneous` | GET | Статус 200 |
| `/users` | GET | Статус 200, тело — массив, наличие пользователя `john@mail.com` |
| `POST /users/` | POST | Статус 201, тело — объект со всеми обязательными полями |

---

## Требования

- [Node.js](https://nodejs.org/) v18+
- npm

Newman устанавливается как локальная зависимость — глобальная установка не нужна.

---

## Установка

```bash
npm install
```

---

## Запуск тестов

### Базовый запуск

```bash
npx newman run tests/fakeapi.platzi.com.postman_collection.json
```

### С подробным выводом

```bash
npx newman run tests/fakeapi.platzi.com.postman_collection.json --verbose
```

### С HTML-отчётом

Сначала установите reporter:

```bash
npm install -D newman-reporter-htmlextra
```

Затем запустите:

```bash
npx newman run tests/fakeapi.platzi.com.postman_collection.json \
  -r htmlextra \
  --reporter-htmlextra-export reports/report.html
```

### С переменными окружения (environment file)

```bash
npx newman run tests/fakeapi.platzi.com.postman_collection.json \
  -e environment/local.json
```

---

## Структура проекта

```
postman-tests/
├── tests/
│   └── fakeapi.platzi.com.postman_collection.json   # Postman-коллекция
├── package.json
└── README.md
```

---

## Полезные флаги Newman

| Флаг | Описание |
|---|---|
| `--iteration-count N` / `-n N` | Запустить коллекцию N раз |
| `--delay-request ms` | Задержка между запросами (мс) |
| `--timeout-request ms` | Таймаут одного запроса (мс) |
| `--bail` | Остановить выполнение при первом провале |
| `--suppress-exit-code` | Всегда выходить с кодом 0 (не падать в CI) |
| `-r cli,json` | Несколько reporters одновременно |

Полная документация: `npx newman run --help`
