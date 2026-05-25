# Описание API — TaskFlow (черновик)

> Изучаю REST API. Здесь описываю эндпоинты, которые нужны для фичи уведомлений.
> Проверяю в Postman на тестовом стенде.

Base URL: `https://api.taskflow.app/v1`

Авторизация: `Authorization: Bearer <token>`

---

## Задачи (Tasks)

### GET /tasks
Получить список задач пользователя.

**Query params:**
| Параметр | Тип | Обязательный | Описание |
|---|---|---|---|
| project_id | integer | нет | Фильтр по проекту |
| assignee_id | integer | нет | Фильтр по исполнителю |
| status | string | нет | todo / in_progress / done |
| page | integer | нет | Пагинация (по умолчанию: 1) |
| per_page | integer | нет | Размер страницы (по умолчанию: 20) |

**Пример запроса:**
```
GET /tasks?project_id=42&status=in_progress&page=1
```

**Пример ответа 200:**
```json
{
  "data": [
    {
      "id": 101,
      "title": "Разработать схему уведомлений",
      "status": "in_progress",
      "assignee_id": 7,
      "deadline": "2026-05-30T18:00:00Z",
      "project_id": 42
    }
  ],
  "meta": {
    "total": 45,
    "page": 1,
    "per_page": 20
  }
}
```

---

### POST /tasks
Создать новую задачу.

**Тело запроса:**
```json
{
  "title": "Описать API уведомлений",
  "project_id": 42,
  "assignee_id": 7,
  "deadline": "2026-06-01T12:00:00Z",
  "description": "Подготовить черновик спецификации"
}
```

**Ответ 201:**
```json
{
  "id": 105,
  "title": "Описать API уведомлений",
  "status": "todo",
  "created_at": "2026-05-25T10:00:00Z"
}
```

**Ошибки:**
| Код | Причина |
|---|---|
| 400 | Не передан обязательный параметр title или project_id |
| 403 | У пользователя нет доступа к проекту |

---

### PATCH /tasks/{id}
Обновить задачу (статус, исполнитель, дедлайн).

**Пример — сменить статус:**
```json
{
  "status": "done"
}
```

> При смене статуса система триггерит событие `TASK_STATUS_CHANGED` → отправка in-app уведомления.

---

## Уведомления (Notifications)

### GET /notifications
Получить список in-app уведомлений текущего пользователя.

**Пример ответа:**
```json
{
  "data": [
    {
      "id": 55,
      "type": "TASK_ASSIGNED",
      "text": "Вам назначена задача: Описать API уведомлений",
      "is_read": false,
      "task_id": 105,
      "created_at": "2026-05-25T10:01:00Z"
    }
  ],
  "unread_count": 3
}
```

---

### POST /notifications/read-all
Отметить все уведомления как прочитанные.

**Ответ 204:** No Content

---

## Настройки уведомлений (Notification Settings)

### GET /me/notification-settings
Получить текущие настройки пользователя.

### PATCH /me/notification-settings
Обновить настройки.

**Пример:**
```json
{
  "email_task_assigned": true,
  "email_deadline_reminder": true,
  "email_digest_mode": "daily",
  "email_digest_time": "09:00"
}
```

---

*Вопросы себе: как обработать timezone у пользователя? Нужно ли отдельное поле в /users или в /me/notification-settings?*
