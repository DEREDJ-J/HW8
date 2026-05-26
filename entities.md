## 1. User (Пользователь)

| Поле          | Тип          | Описание                   |
| ------------- | ------------ | -------------------------- |
| id            | SERIAL       | PK                         |
| email         | VARCHAR(255) | unique                     |
| password_hash | VARCHAR(255) |                            |
| full_name     | VARCHAR(100) |                            |
| settings      | JSONB        | настройки уведомлений и AI |
| created_at    | TIMESTAMP    |                            |

## 2. Task (Задача)

| Поле            | Тип          | Описание                                          |
| --------------- | ------------ | ------------------------------------------------- |
| id              | SERIAL       | PK                                                |
| user_id         | INTEGER      | FK → User                                         |
| parent_task_id  | INTEGER      | FK → Task (подзадачи)                             |
| title           | VARCHAR(200) |                                                   |
| description     | TEXT         |                                                   |
| estimated_hours | DECIMAL(5,2) |                                                   |
| actual_hours    | DECIMAL(5,2) |                                                   |
| status          | VARCHAR(20)  | todo/in_progress/stuck/review/completed/cancelled |
| deadline        | TIMESTAMP    |                                                   |
| created_at      | TIMESTAMP    |                                                   |
| completed_at    | TIMESTAMP    |                                                   |

## 3. TaskHistory (История для AI)

| Поле              | Тип          | Описание  |
| ----------------- | ------------ | --------- |
| id                | SERIAL       | PK        |
| user_id           | INTEGER      | FK → User |
| task_id           | INTEGER      | FK → Task |
| estimated_hours   | DECIMAL(5,2) |           |
| actual_hours      | DECIMAL(5,2) |           |
| deviation_percent | DECIMAL(5,2) |           |
| completed_at      | TIMESTAMP    |           |

## 4. TimeSlot (Временной слот)

| Поле       | Тип         | Описание                            |
| ---------- | ----------- | ----------------------------------- |
| id         | SERIAL      | PK                                  |
| user_id    | INTEGER     | FK → User                           |
| task_id    | INTEGER     | FK → Task (nullable, один к одному) |
| start_time | TIMESTAMP   |                                     |
| end_time   | TIMESTAMP   |                                     |
| type       | VARCHAR(20) | busy/free/suggested                 |

## 5. AIRecommendation (AI-рекомендации)

| Поле       | Тип         | Описание                                            |
| ---------- | ----------- | --------------------------------------------------- |
| id         | SERIAL      | PK                                                  |
| user_id    | INTEGER     | FK → User                                           |
| task_id    | INTEGER     | FK → Task (nullable)                                |
| type       | VARCHAR(30) | slot_suggestion/breakup/warning/estimate_correction |
| content    | JSONB       | данные рекомендации                                 |
| status     | VARCHAR(20) | pending/accepted/rejected                           |
| created_at | TIMESTAMP   |                                                     |

## 6. Notification (Уведомления)

| Поле       | Тип         | Описание                                      |
| ---------- | ----------- | --------------------------------------------- |
| id         | SERIAL      | PK                                            |
| user_id    | INTEGER     | FK → User                                     |
| task_id    | INTEGER     | FK → Task (nullable)                          |
| type       | VARCHAR(30) | stuck_warning/deadline_reminder/ai_suggestion |
| message    | TEXT        |                                               |
| is_read    | BOOLEAN     |                                               |
| created_at | TIMESTAMP   |                                               |

## 7. WeeklyReport (Еженедельный отчёт)

| Поле         | Тип       | Описание                             |
| ------------ | --------- | ------------------------------------ |
| id           | SERIAL    | PK                                   |
| user_id      | INTEGER   | FK → User                            |
| week_start   | DATE      |                                      |
| data         | JSONB     | метрики точности, завершённые задачи |
| generated_at | TIMESTAMP |                                      |
