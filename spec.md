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



# Завдання 2: Вимоги та Use Cases (EchoLink)

## 1. Актори та їхні цілі
* **Користувач (User):** Авторизований учасник. Мета — обмінюватися повідомленнями.
* **Адміністратор (Admin):** Спадкує User. Мета — модерувати чат-кімнати.

## 2. Сценарії Gherkin для UC: "Відправка повідомлення"

**Сценарій 1: Успішна відправка повідомлення (Happy path)**
* **Given** користувач "Ілля" авторизований і знаходиться в кімнаті "General"
* **When** він вводить текст "Привіт" і натискає Enter
* **Then** повідомлення з'являється в історії чату "General" з його авторством

**Сценарій 2: Спроба відправки довгого тексту (Alternate path)**
* **Given** користувач "Ілля" знаходиться в кімнаті "General"
* **When** він намагається відправити повідомлення на 1500 символів
* **Then** система відхиляє відправку
* **And** показує помилку "Перевищено ліміт"

**Сценарій 3: Блокування (Error path)**
* **Given** користувача "Ілля" заблоковано в кімнаті "Dev"
* **When** він намагається відправити туди повідомлення
* **Then** система відхиляє відправку з помилкою "Access denied"

## 3. Функціональні вимоги (EARS)
* **REQ-F01:** WHEN a User submits a valid message to an active room, the system SHALL broadcast this message to all connected clients in that room.
* **REQ-F02:** WHEN a User submits a message exceeding 1000 characters, the system SHALL reject it and display a validation error.
* **REQ-F03:** WHERE a User does not have active membership in a ChatRoom, the system SHALL prevent them from sending messages.

## 4. Нефункціональні вимоги (NFR)
* **NFR-P01 (Performance):** Час доставки повідомлення не повинен перевищувати **200 мс** при навантаженні до **500** активних користувачів.
* **NFR-S01 (Security):** Трафік повинен бути зашифрований за протоколом **TLS 1.2+**.
* **NFR-U01 (Usability):** Інтерфейс повинен коректно працювати на терміналах розміром не менше **80x24** символів.

## 5. Поза обсягом (Out of Scope)
* **Відправка файлів:** Не реалізується в текстовому термінальному MVP.

## 6. Матриця трасування
| ID Вимоги | Зв'язок (Сценарій / UC) |
| :--- | :--- |
| REQ-F01 | UC: Send Message (Сценарій 1 - Успішна відправка) |
| REQ-F02 | UC: Send Message (Сценарій 2 - Перевищення ліміту) |
| REQ-F03 | UC: Send Message (Сценарій 3 - Блокування) |