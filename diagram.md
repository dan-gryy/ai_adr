erDiagram
    User {
        Int id PK
        String username
        String email
    }
    ChatRoom {
        Int id PK
        String name
        Timestamp created_at
    }
    Message {
        Int id PK
        String content
        Timestamp sent_at
        Int author_id FK
        Int room FK
    }
    User_ChatRoom {
        Int user_id FK
        Int room_id FK
    }

    User ||--o{ Message : "writes"
    ChatRoom ||--o{ Message : "contains"
    User ||--o{ User_ChatRoom : "has"
    ChatRoom ||--o{ User_ChatRoom : "has"