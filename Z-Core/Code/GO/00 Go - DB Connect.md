2025020221:34
___
Date: 02-02-2025 | 21:34
Tags: #go #db #connect #init
mapofcontents:
___
## DB drivers and connect

### PostgresQL

```go
package main

import (
	"context"
	"database/sql"
	"fmt"
	"log"

	"github.com/jackc/pgx/v4"
)

func main() {
	// Формируем DSN (Data Source Name)
	dsn := "postgres://postgres:1111@127.0.0.1:5431/testDB?sslmode=disable"
	ctx := context.Background()

	// Подключаемся к базе данных
	db, err := pgx.Connect(ctx, dsn)
	if err != nil {
		log.Fatalf("Failed to connect to database: %v", err)
	}
	defer db.Close(ctx)

	// Проверяем подключение
	err = db.Ping(ctx)
	if err != nil {
		log.Fatalf("Failed to ping database: %v", err)
	}

	fmt.Println("Successfully connected to PostgreSQL!")

	// Пример: Создание таблицы
	createTableSQL := `
    CREATE TABLE IF NOT EXISTS users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(50) NOT NULL,
        age INT NOT NULL
    );`
	_, err = db.Exec(ctx, createTableSQL)
	if err != nil {
		log.Fatalf("Failed to create table: %v", err)
	}

	fmt.Println("Table 'users' created or already exists.")

	// Вставка данных с RETURNING
	var lastInsertID int
	insertSQL := "INSERT INTO users (name, age) VALUES ($1, $2) RETURNING id"
	err = db.QueryRow(ctx, insertSQL, "Alice", 30).Scan(&lastInsertID)
	if err != nil {
		log.Fatalf("Failed to insert data: %v", err)
	}

	fmt.Printf("Inserted user with ID: %d\n", lastInsertID)

	// Пример: Чтение данных
	var (
		id   int
		name string
		age  int
	)
	selectSQL := "SELECT id, name, age FROM users WHERE id = $1"
	err = db.QueryRow(ctx, selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err != nil {
		log.Fatalf("Failed to query data: %v", err)
	}
	fmt.Printf("User: ID=%d, Name=%s, Age=%d\n", id, name, age)

	// Пример: Обновление данных
	updateSQL := "UPDATE users SET age = $1 WHERE id = $2"
	_, err = db.Exec(ctx, updateSQL, 31, lastInsertID)
	if err != nil {
		log.Fatalf("Failed to update data: %v", err)
	}
	fmt.Println("User age updated.")

	// Проверяем обновление
	err = db.QueryRow(ctx, selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err != nil {
		log.Fatalf("Failed to query updated data: %v", err)
	}
	fmt.Printf("Updated User: ID=%d, Name=%s, Age=%d\n", id, name, age)

	// Пример: Удаление данных
	deleteSQL := "DELETE FROM users WHERE id = $1"
	_, err = db.Exec(ctx, deleteSQL, lastInsertID)
	if err != nil {
		log.Fatalf("Failed to delete data: %v", err)
	}
	fmt.Println("User deleted.")

	// Проверяем удаление
	err = db.QueryRow(ctx, selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err == sql.ErrNoRows {
		fmt.Println("User not found (deleted successfully).")
	} else if err != nil {
		log.Fatalf("Failed to query deleted data: %v", err)
	}
}

```

### MySQL

```go
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/go-sql-driver/mysql"
)

func main() {
	// Формируем DSN (Data Source Name)
	dsn := "milk:1111@tcp(127.0.0.1:3306)/testDB?parseTime=true"

	// Подключаемся к базе данных
	db, err := sql.Open("mysql", dsn)
	if err != nil {
		log.Fatalf("Failed to connect to database: %v", err)
	}
	defer db.Close()

	// Проверяем подключение
	err = db.Ping()
	if err != nil {
		log.Fatalf("Failed to ping database: %v", err)
	}

	fmt.Println("Successfully connected to MySQL!")

	// Пример: Создание таблицы
	createTableSQL := `
    CREATE TABLE IF NOT EXISTS users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        name VARCHAR(50) NOT NULL,
        age INT NOT NULL
    );`
	_, err = db.Exec(createTableSQL)
	if err != nil {
		log.Fatalf("Failed to create table: %v", err)
	}

	fmt.Println("Table 'users' created or already exists.")

	// Пример: Вставка данных
	insertSQL := "INSERT INTO users (name, age) VALUES (?, ?)"
	result, err := db.Exec(insertSQL, "Alice", 30)
	if err != nil {
		log.Fatalf("Failed to insert data: %v", err)
	}

	lastInsertID, err := result.LastInsertId()
	if err != nil {
		log.Fatalf("Failed to get last insert ID: %v", err)
	}
	fmt.Printf("Inserted user with ID: %d\n", lastInsertID)

	// Пример: Чтение данных
	var (
		id   int
		name string
		age  int
	)
	selectSQL := "SELECT id, name, age FROM users WHERE id = ?"
	err = db.QueryRow(selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err != nil {
		log.Fatalf("Failed to query data: %v", err)
	}
	fmt.Printf("User: ID=%d, Name=%s, Age=%d\n", id, name, age)

	// Пример: Обновление данных
	updateSQL := "UPDATE users SET age = ? WHERE id = ?"
	_, err = db.Exec(updateSQL, 31, lastInsertID)
	if err != nil {
		log.Fatalf("Failed to update data: %v", err)
	}
	fmt.Println("User age updated.")

	// Проверяем обновление
	err = db.QueryRow(selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err != nil {
		log.Fatalf("Failed to query updated data: %v", err)
	}
	fmt.Printf("Updated User: ID=%d, Name=%s, Age=%d\n", id, name, age)

	// Пример: Удаление данных
	deleteSQL := "DELETE FROM users WHERE id = ?"
	_, err = db.Exec(deleteSQL, lastInsertID)
	if err != nil {
		log.Fatalf("Failed to delete data: %v", err)
	}
	fmt.Println("User deleted.")

	// Проверяем удаление
	err = db.QueryRow(selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err == sql.ErrNoRows {
		fmt.Println("User not found (deleted successfully).")
	} else if err != nil {
		log.Fatalf("Failed to query deleted data: %v", err)
	}
}
```

### Sqlite

```go
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/mattn/go-sqlite3"
)

func main() {

	// Подключаемся к базе данных
	db, err := sql.Open("sqlite3", "./testDB.db")
	if err != nil {
		log.Fatalf("Failed to connect to database: %v", err)
	}
	defer db.Close()

	// Проверяем подключение
	err = db.Ping()
	if err != nil {
		log.Fatalf("Failed to ping database: %v", err)
	}

	fmt.Println("Successfully connected to SQLite!")

	// Пример: Создание таблицы
	createTableSQL := `
      CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        age INTEGER NOT NULL
      );`

	_, err = db.Exec(createTableSQL)
	if err != nil {
		log.Fatalf("Failed to create table: %v", err)
	}

	fmt.Println("Table 'users' created or already exists.")

	// Пример: Вставка данных
	insertSQL := "INSERT INTO users (name, age) VALUES (?, ?)"
	result, err := db.Exec(insertSQL, "Alice", 30)
	if err != nil {
		log.Fatalf("Failed to insert data: %v", err)
	}

	lastInsertID, err := result.LastInsertId()
	if err != nil {
		log.Fatalf("Failed to get last insert ID: %v", err)
	}
	fmt.Printf("Inserted user with ID: %d\n", lastInsertID)

	// Пример: Чтение данных
	var (
		id   int
		name string
		age  int
	)
	selectSQL := "SELECT id, name, age FROM users WHERE id = ?"
	err = db.QueryRow(selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err != nil {
		log.Fatalf("Failed to query data: %v", err)
	}
	fmt.Printf("User: ID=%d, Name=%s, Age=%d\n", id, name, age)

	// Пример: Обновление данных
	updateSQL := "UPDATE users SET age = ? WHERE id = ?"
	_, err = db.Exec(updateSQL, 31, lastInsertID)
	if err != nil {
		log.Fatalf("Failed to update data: %v", err)
	}
	fmt.Println("User age updated.")

	// Проверяем обновление
	err = db.QueryRow(selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err != nil {
		log.Fatalf("Failed to query updated data: %v", err)
	}
	fmt.Printf("Updated User: ID=%d, Name=%s, Age=%d\n", id, name, age)

	// Пример: Удаление данных
	deleteSQL := "DELETE FROM users WHERE id = ?"
	_, err = db.Exec(deleteSQL, lastInsertID)
	if err != nil {
		log.Fatalf("Failed to delete data: %v", err)
	}
	fmt.Println("User deleted.")

	// Проверяем удаление
	err = db.QueryRow(selectSQL, lastInsertID).Scan(&id, &name, &age)
	if err == sql.ErrNoRows {
		fmt.Println("User not found (deleted successfully).")
	} else if err != nil {
		log.Fatalf("Failed to query deleted data: %v", err)
	}
}
```

-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
