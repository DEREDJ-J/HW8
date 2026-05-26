---

## Файл 2: `entities.md`

```markdown
# Сущности Planly (с внешними ключами)

## 1. User (Пользователь)

| Поле          | Тип          | Обязательное | Описание                   |
| ------------- | ------------ | ------------ | -------------------------- |
| id            | SERIAL       | Да           | PK                         |
| email         | VARCHAR(255) | Да           | unique                     |
| password_hash | VARCHAR(255) | Да           |                            |
| full_name     | VARCHAR(100) | Да           |                            |
| settings      | JSONB        | Нет          | настройки уведомлений и AI |
| created_at    | TIMESTAMP    | Да           | default: NOW()             |

## 2. Task (Задача)

| Поле            | Тип          | Обязательное | FK        | Описание                                          |
| --------------- | ------------ | ------------ | --------- | ------------------------------------------------- |
| id              | SERIAL       | Да           | PK        |                                                   |
| user_id         | INTEGER      | Да           | → User.id | владелец задачи                                   |
| parent_task_id  | INTEGER      | Нет          | → Task.id | родительская задача (для подзадач)                |
| title           | VARCHAR(200) | Да           |           |                                                   |
| description     | TEXT         | Нет          |           |                                                   |
| estimated_hours | DECIMAL(5,2) | Да           |           | default: 0                                        |
| actual_hours    | DECIMAL(5,2) | Нет          |           |                                                   |
| status          | VARCHAR(20)  | Да           |           | todo/in_progress/stuck/review/completed/cancelled |
| deadline        | TIMESTAMP    | Нет          |           |                                                   |
| created_at      | TIMESTAMP    | Да           |           | default: NOW()                                    |
| completed_at    | TIMESTAMP    | Нет          |           |                                                   |

## 3. TaskHistory (История для AI)

| Поле              | Тип          | Обязательное | FK        | Описание                              |
| ----------------- | ------------ | ------------ | --------- | ------------------------------------- |
| id                | SERIAL       | Да           | PK        |                                       |
| user_id           | INTEGER      | Да           | → User.id |                                       |
| task_id           | INTEGER      | Да           | → Task.id |                                       |
| estimated_hours   | DECIMAL(5,2) | Да           |           |                                       |
| actual_hours      | DECIMAL(5,2) | Да           |           |                                       |
| deviation_percent | DECIMAL(5,2) | Нет          |           | (actual - estimated)/estimated \* 100 |
| completed_at      | TIMESTAMP    | Да           |           |                                       |

## 4. TimeSlot (Временной слот)

| Поле       | Тип         | Обязательное | FK        | Описание              |
| ---------- | ----------- | ------------ | --------- | --------------------- |
| id         | SERIAL      | Да           | PK        |                       |
| user_id    | INTEGER     | Да           | → User.id |                       |
| task_id    | INTEGER     | Нет          | → Task.id | NULL = свободный слот |
| start_time | TIMESTAMP   | Да           |           |                       |
| end_time   | TIMESTAMP   | Да           |           |                       |
| type       | VARCHAR(20) | Да           |           | busy/free/suggested   |

## 5. AIRecommendation (AI-рекомендации)

| Поле       | Тип         | Обязательное | FK        | Описание                                            |
| ---------- | ----------- | ------------ | --------- | --------------------------------------------------- |
| id         | SERIAL      | Да           | PK        |                                                     |
| user_id    | INTEGER     | Да           | → User.id |                                                     |
| task_id    | INTEGER     | Нет          | → Task.id |                                                     |
| type       | VARCHAR(30) | Да           |           | slot_suggestion/breakup/warning/estimate_correction |
| content    | JSONB       | Да           |           | данные рекомендации                                 |
| status     | VARCHAR(20) | Да           |           | pending/accepted/rejected, default: pending         |
| created_at | TIMESTAMP   | Да           |           | default: NOW()                                      |

## 6. Notification (Уведомления)

| Поле       | Тип         | Обязательное | FK        | Описание                                      |
| ---------- | ----------- | ------------ | --------- | --------------------------------------------- |
| id         | SERIAL      | Да           | PK        |                                               |
| user_id    | INTEGER     | Да           | → User.id |                                               |
| task_id    | INTEGER     | Нет          | → Task.id |                                               |
| type       | VARCHAR(30) | Да           |           | stuck_warning/deadline_reminder/ai_suggestion |
| message    | TEXT        | Да           |           |                                               |
| is_read    | BOOLEAN     | Да           |           | default: false                                |
| created_at | TIMESTAMP   | Да           |           | default: NOW()                                |

## 7. WeeklyReport (Еженедельный отчёт)

| Поле         | Тип       | Обязательное | FK        | Описание                             |
| ------------ | --------- | ------------ | --------- | ------------------------------------ |
| id           | SERIAL    | Да           | PK        |                                      |
| user_id      | INTEGER   | Да           | → User.id |                                      |
| week_start   | DATE      | Да           |           |                                      |
| data         | JSONB     | Да           |           | метрики точности, завершённые задачи |
| generated_at | TIMESTAMP | Да           |           | default: NOW()                       |

## Сводка внешних ключей

| Таблица          | Внешний ключ   | Ссылается на |
| ---------------- | -------------- | ------------ |
| Task             | user_id        | User.id      |
| Task             | parent_task_id | Task.id      |
| TaskHistory      | user_id        | User.id      |
| TaskHistory      | task_id        | Task.id      |
| TimeSlot         | user_id        | User.id      |
| TimeSlot         | task_id        | Task.id      |
| AIRecommendation | user_id        | User.id      |
| AIRecommendation | task_id        | Task.id      |
| Notification     | user_id        | User.id      |
| Notification     | task_id        | Task.id      |
| WeeklyReport     | user_id        | User.id      |
```
