# Модуль 4 — Анализ API и интеграций

## Что такое API?

**API** (Application Programming Interface) — набор правил, по которым одна программа общается с другой.

**REST API** — наиболее распространённый стиль, основанный на HTTP.

---

## HTTP-методы

| Метод | Назначение | Пример |
|---|---|---|
| GET | Получение данных | GET /users/123 |
| POST | Создание ресурса | POST /users |
| PUT | Полное обновление | PUT /users/123 |
| PATCH | Частичное обновление | PATCH /users/123 |
| DELETE | Удаление | DELETE /users/123 |

---

## HTTP Статус-коды

| Код | Группа | Значение |
|---|---|---|
| 200 | 2xx — Успех | OK |
| 201 | 2xx — Успех | Created |
| 204 | 2xx — Успех | No Content |
| 400 | 4xx — Ошибка клиента | Bad Request |
| 401 | 4xx — Ошибка клиента | Unauthorized |
| 403 | 4xx — Ошибка клиента | Forbidden |
| 404 | 4xx — Ошибка клиента | Not Found |
| 422 | 4xx — Ошибка клиента | Unprocessable Entity |
| 500 | 5xx — Ошибка сервера | Internal Server Error |
| 503 | 5xx — Ошибка сервера | Service Unavailable |

---

## Структура REST запроса

```
POST /api/v1/orders HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGc...
Content-Type: application/json

{
  "product_id": 42,
  "quantity": 2,
  "user_id": 1001
}
```

**Компоненты:**
- **Endpoint (URL)** — адрес ресурса
- **Headers** — метаданные запроса (авторизация, тип контента)
- **Body** — тело запроса (для POST/PUT/PATCH)
- **Query Parameters** — фильтры в URL: `/users?role=admin&page=1`

---

## Swagger / OpenAPI

**OpenAPI** — стандарт описания REST API.
**Swagger UI** — визуальный интерфейс для чтения и тестирования API.

**Что смотреть в Swagger:**
1. Список эндпоинтов (пути)
2. HTTP-методы для каждого эндпоинта
3. Параметры запроса (path, query, header, body)
4. Схемы данных (модели)
5. Примеры запросов и ответов
6. Коды ответов и их описание

---

## Postman — базовые операции

1. Создать новый запрос: New → HTTP Request
2. Выбрать метод (GET, POST...)
3. Ввести URL
4. Добавить Headers (Content-Type, Authorization)
5. Добавить Body (для POST/PUT)
6. Нажать Send
7. Проанализировать ответ

**Коллекции:** группировка запросов по проекту/модулю

---

## Интеграционные паттерны

| Паттерн | Описание |
|---|---|
| Синхронный REST | Запрос-ответ в реальном времени |
| Асинхронный (очереди) | Через брокер (Kafka, RabbitMQ) |
| Webhook | Сервер уведомляет клиента о событии |
| GraphQL | Клиент запрашивает только нужные поля |

---

## Роль SA при работе с API

- Описывать требования к API-эндпоинтам
- Согласовывать контракт API с разработчиками
- Документировать API в Swagger/Confluence
- Проверять работу API в Postman
- Описывать обработку ошибок и edge cases
