# Намір
Створення концептуальної ER-моделі для чат-застосунку EchoLink.

# Сутності та атрибути
* **User:** id PK, username, email
* **ChatRoom:** id PK, name, created_at
* **Message:** id PK, content, sent_at, author_id FK, room FK

# Зв'язки
1. User ||--o{ Message : "writes"
2. ChatRoom ||--o{ Message : "contains"
3. User }o--o{ ChatRoom : "participates in"

# Критерії прийняття
* Модель знаходиться у 3NF.