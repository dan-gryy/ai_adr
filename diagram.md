# Діаграми послідовності (UC: «Відправка повідомлення»)

## Сценарій 1: Успішна відправка повідомлення (Happy path)

```mermaid
sequenceDiagram
    actor Client
    participant TCPServer
    participant Database
    actor OtherClients

    Client ->> TCPServer: sendMessage(chatId, userId, payload)
    activate TCPServer

    TCPServer ->> Database: verifyUserAccess(chatId, userId)
    activate Database
    Database -->> TCPServer: accessStatus = true
    deactivate Database

    TCPServer ->> Database: insertMessage(chatId, userId, payload)
    activate Database
    Database -->> TCPServer: messageId
    deactivate Database

    loop Для кожного активного учасника кімнати (крім відправника)
        TCPServer -) OtherClients: pushEvent(messageId, userId, payload)
    end

    TCPServer -->> Client: 200 OK (messageId)
    deactivate TCPServer
```

## Сценарій 2: Перевищення ліміту довжини повідомлення (Alternate path)

```mermaid
sequenceDiagram
    actor Client
    participant TCPServer
    participant Database

    Client ->> TCPServer: sendMessage(chatId, userId, payload)
    activate TCPServer

    TCPServer ->> TCPServer: validateLength(payload)

    break Довжина повідомлення понад 1000 символів
        opt Підозріла спам-активність (довжина понад 5000 символів)
            TCPServer ->> Database: logSuspiciousActivity(userId, chatId, length)
            activate Database
            Database -->> TCPServer: logged
            deactivate Database
        end
        TCPServer -->> Client: 400 Bad Request (Limit exceeded)
    end

    deactivate TCPServer
```

## Сценарій 3: Відправка повідомлення заблокованим користувачем (Error path)

```mermaid
sequenceDiagram
    actor Client
    participant TCPServer
    participant Database

    Client ->> TCPServer: sendMessage(chatId, userId, payload)
    activate TCPServer

    TCPServer ->> Database: verifyUserAccess(chatId, userId)
    activate Database
    Database -->> TCPServer: accessStatus = false
    deactivate Database

    break Користувач заблокований або не є учасником кімнати
        TCPServer -->> Client: 403 Forbidden (Access denied)
    end

    deactivate TCPServer
```