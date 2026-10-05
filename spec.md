# Намір
Створення концептуальної ER-моделі для чат-застосунку EchoLink.

# Сутності та атрибути
* **User:** id (UUID) PK, username, email
* **ChatRoom:** id (UUID) PK, name, created_at
* **Message:** id (UUID) PK, content, sent_at, author_id (UUID) FK, room_id (UUID) FK

# Зв'язки
1. User ||--o{ Message : "writes"
2. ChatRoom ||--o{ Message : "contains"
3. User }o--o{ ChatRoom : "participates in"

# Критерії прийняття
* [x] Модель знаходиться у 3NF.
* [x] Для всіх первинних і зовнішніх ключів використовується суворо тип `UUID` (не Int).
* [x] Зв'язок M:N між User та ChatRoom змодельований без фізичних сполучних таблиць.
* [x] Точний неймінг зовнішніх ключів у Message: `author_id` та `room_id`.