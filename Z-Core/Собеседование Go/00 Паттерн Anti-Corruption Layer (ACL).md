2025030700:05
___
Date: 07-03-2025 | 00:05
Tags: #
mapofcontents: [[zero-links|OO Links]]
___
## Паттерн Anti-Corruption Layer (ACL)

> **Anti-Corruption Layer (ACL)** — это архитектурный паттерн, который используется для защиты одной системы (или её подсистемы) от влияния другой системы с несовместимыми или нежелательными моделями данных, API или логикой. Основная идея — создать промежуточный слой, который изолирует "чистую" доменную модель вашей системы от "чужой" или устаревшей системы, предотвращая "загрязнение" (corruption) кода.

> [!NOTE] FAQ
> В контексте Go этот паттерн особенно полезен при интеграции с внешними сервисами, устаревшими системами (legacy) или сторонними API.

---
### Что такое Anti-Corruption Layer?

- **Цель**: Сохранить целостность внутренней модели данных и бизнес-логики, изолировав её от внешних систем.
- **Когда использовать**:
    - Интеграция с внешним API, которое возвращает данные в неудобном формате.
    - Работа с устаревшей системой, где изменение её кода невозможно.
    - Защита доменной логики от изменений во внешних зависимостях.
- **Как работает**: ACL переводит данные из внешней системы в формат, понятный внутренней, и наоборот, выступая "переводчиком" и барьером.

---
### Пример ситуации

Предположим, ваша система использует чистую модель пользователя:
```go
type User struct {
    ID    int
    Name  string
    Email string
}
```

Но вы интегрируетесь с внешним API, которое возвращает данные в формате:
```json
{
    "user_id": "123",
    "full_name": "John Doe",
    "contact_email": "john@example.com",
    "legacy_field": "some_old_data"
}
```

Прямое использование этой структуры "загрязнит" вашу модель лишними полями и несовместимыми именами.

---
### Реализация Anti-Corruption Layer в Go

#### Шаг 1: Определите внешнюю модель

Создаем структуру для данных внешнего API, не смешивая её с внутренней моделью:
```go
type LegacyUser struct {
    UserID       string `json:"user_id"`
    FullName     string `json:"full_name"`
    ContactEmail string `json:"contact_email"`
    LegacyField  string `json:"legacy_field"`
}
```

#### Шаг 2: Создайте внутреннюю модель

Это ваша "чистая" доменная модель:
```go
type User struct {
    ID    int
    Name  string
    Email string
}
```

#### Шаг 3: Реализуйте ACL

Создаем слой, который преобразует данные между моделями:
```go
type UserRepository interface {
    GetUser(id string) (User, error)
}

type LegacyUserService struct {
    // Здесь может быть клиент для внешнего API
}

func (s LegacyUserService) GetUser(id string) (User, error) {
    // Симуляция вызова внешнего API
    legacyUser := LegacyUser{
        UserID:       id,
        FullName:     "John Doe",
        ContactEmail: "john@example.com",
        LegacyField:  "some_old_data",
    }

    // Преобразование в чистую модель (ACL в действии)
    userID, err := strconv.Atoi(legacyUser.UserID)
    if err != nil {
        return User{}, fmt.Errorf("invalid user ID: %v", err)
    }

    return User{
        ID:    userID,
        Name:  legacyUser.FullName,
        Email: legacyUser.ContactEmail,
    }, nil
}
```

#### Шаг 4: Используйте в бизнес-логике

Ваша основная логика работает только с чистой моделью:
```go
type UserService struct {
    repo UserRepository
}

func (s UserService) ProcessUser(id string) error {
    user, err := s.repo.GetUser(id)
    if err != nil {
        return err
    }
    fmt.Printf("Processing user: %s (ID: %d, Email: %s)\n", user.Name, user.ID, user.Email)
    return nil
}

func main() {
    legacyService := LegacyUserService{}
    service := UserService{repo: legacyService}
    service.ProcessUser("123")
    // Вывод: Processing user: John Doe (ID: 123, Email: john@example.com)
}
```

---
### Как это работает?

1. **Внешний слой**: `LegacyUserService` взаимодействует с внешней системой и получает данные в её формате (`LegacyUser`).
2. **ACL**: В методе `GetUser` происходит преобразование из `LegacyUser` в `User`, отбрасывая ненужное (`LegacyField`) и адаптируя поля (например, `UserID` в `ID`).
3. **Внутренний слой**: `UserService` работает только с чистой моделью `User`, не зная о внешнем формате.

---

### Преимущества ACL

- **Изоляция**: Внутренняя модель не зависит от внешней системы.
- **Гибкость**: Изменения во внешнем API требуют правок только в ACL, а не в бизнес-логике.
- **Чистота кода**: Доменная модель остается простой и понятной.

---
### Когда не использовать?

- Если внешняя система полностью совместима с вашей моделью, ACL может быть избыточным (нарушение KISS).
- Для простых интеграций с минимальными данными преобразование может быть лишним.

---
### Итог

**Anti-Corruption Layer** в Go — это прослойка, которая защищает вашу систему от "чужого" кода или данных, обеспечивая чистоту доменной модели. В примере выше ACL реализован через интерфейс `UserRepository` и преобразование данных в `LegacyUserService`. Это особенно полезно в проектах с интеграциями или legacy-кодом.


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
