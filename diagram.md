erDiagram
    User {
        UUID id PK
        String username
        String email
    }
    ChatRoom {
        UUID id PK
        String name
        Timestamp created_at
    }
    Message {
        UUID id PK
        String content
        Timestamp sent_at
        UUID author_id FK
        UUID room_id FK
    }

    User ||--o{ Message : "writes"
    ChatRoom ||--o{ Message : "contains"
    User }o--o{ ChatRoom : "participates in"