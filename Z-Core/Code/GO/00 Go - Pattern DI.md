2025020406:05
___
Date: 04-02-2025 | 06:05
Tags: #go #pattern #di 
mapofcontents:
___
## Pattern Dependency Injection (Внедрение зависимостей)

### Когда использовать?

Когда нужно **разделить логику создания зависимостей и их использование**. Это делает код более тестируемым и гибким.

Go не поддерживает автоматический DI, как в Spring (Java) или Dagger (Kotlin), но можно внедрять зависимости вручную через **параметры конструктора**.

**Пример Dependency Injection в Go**
```go
package main

import "fmt"

// Интерфейс репозитория (интерфейс зависимости)
type UserRepository interface {
	GetUserName(id int) string
}

// Реализация репозитория
type PostgresUserRepository struct{}

func (r *PostgresUserRepository) GetUserName(id int) string {
	return fmt.Sprintf("User%d (из Postgres)", id)
}

// Сервис, использующий DI
type UserService struct {
	repo UserRepository
}

// Конструктор с DI
func NewUserService(repo UserRepository) *UserService {
	return &UserService{repo: repo}
}

func (s *UserService) GetUserName(id int) string {
	return s.repo.GetUserName(id)
}

func main() {
	// Внедряем зависимость
	repo := &PostgresUserRepository{}
	service := NewUserService(repo)

	fmt.Println(service.GetUserName(42)) // User42 (из Postgres)
}
```

**Отличие от других языков:**
- В Go DI делается **вручную**, передавая зависимости через **конструктор структуры**.
- В продакшене часто используют **Fx или Wire** для автоматического DI.

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
- [[ml Go Patterns]]

------
**Links** (Внешние ссылки)
-
