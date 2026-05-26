erDiagram
User {
int id PK
string email
string password_hash
string full_name
jsonb settings
timestamp created_at
}

    Task {
        int id PK
        int user_id FK
        int parent_task_id FK
        string title
        text description
        decimal estimated_hours
        decimal actual_hours
        string status
        timestamp deadline
        timestamp created_at
        timestamp completed_at
    }

    TaskHistory {
        int id PK
        int user_id FK
        int task_id FK
        decimal estimated_hours
        decimal actual_hours
        decimal deviation_percent
        timestamp completed_at
    }

    TimeSlot {
        int id PK
        int user_id FK
        int task_id FK
        timestamp start_time
        timestamp end_time
        string type
    }

    AIRecommendation {
        int id PK
        int user_id FK
        int task_id FK
        string type
        jsonb content
        string status
        timestamp created_at
    }

    Notification {
        int id PK
        int user_id FK
        int task_id FK
        string type
        text message
        boolean is_read
        timestamp created_at
    }

    WeeklyReport {
        int id PK
        int user_id FK
        date week_start
        jsonb data
        timestamp generated_at
    }

    User ||--o{ Task : "has"
    User ||--o{ TaskHistory : "has"
    User ||--o{ TimeSlot : "has"
    User ||--o{ AIRecommendation : "has"
    User ||--o{ Notification : "has"
    User ||--o{ WeeklyReport : "has"

    Task ||--o{ Task : "contains subtasks"
    Task ||--o| TimeSlot : "occupies"
    Task ||--o{ TaskHistory : "has history"
    Task ||--o{ AIRecommendation : "receives"
    Task ||--o{ Notification : "triggers"

у меня только так заработало
