### **1. Основы Java (для повторения)**

- Исключения (try-catch, checked/unchecked exceptions, best practices)
- Коллекции и Generics (List, Set, Map, Queue, Stack, Comparator, Comparable)
- Stream API (filter, map, reduce, collect)
- Многопоточное программирование (Thread, Runnable, Callable, ExecutorService)
- JVM, JIT, GC (как работает сборщик мусора, какие бывают GC)
- Модульность в Java (Jigsaw, Module System)
- Java 8–21: новые фичи (Optional, Records, Sealed classes, Virtual Threads)

---

### **2. Основы SQL и базы данных**

- Реляционные БД: PostgreSQL / MySQL
- Основные SQL-запросы (SELECT, JOIN, GROUP BY, HAVING)
- Индексы и их влияние на производительность
- Транзакции и уровни изоляции (ACID, MVCC)
- Нормализация и денормализация БД

---

### **3. Java I/O, NIO и работа с файлами**

- Основные классы: `InputStream`, `OutputStream`, `Reader`, `Writer`
- Работа с файлами через `File`, `Files`, `BufferedReader`, `BufferedWriter`
- Работа с бинарными файлами (`DataInputStream`, `DataOutputStream`)
- Сериализация и десериализация объектов (`ObjectInputStream`, `ObjectOutputStream`)
- New I/O (NIO): `Path`, `Paths`, `Files`, `ByteBuffer`, `FileChannel`
- Работа с сокетами (`Socket`, `ServerSocket`, `DatagramSocket`)
- Асинхронные операции с NIO (`AsynchronousFileChannel`)

---

### **4. Объектно-ориентированное программирование (ООП) и продвинутые концепции Java**

🔹 **Ключевые принципы ООП**

- Инкапсуляция, наследование, полиморфизм, абстракция
- Разница между `extends` и `implements`

🔹 **Интерфейсы и абстрактные классы**

- Разница между интерфейсами и абстрактными классами
- Интерфейсы с дефолтными методами (`default` methods)
- Функциональные интерфейсы (`@FunctionalInterface`, `Predicate`, `Function`, `Consumer`, `Supplier`)
- Лямбда-выражения и их применение

🔹 **Принципы SOLID, GRASP**

- Разбор каждого принципа SOLID на примерах
- Почему важно их соблюдать в реальных проектах

🔹 **Паттерны проектирования в Java (GOF)**

- Singleton, Factory, Builder
- Proxy, Decorator, Adapter
- Observer, Strategy, Command
- Hexagonal Architecture, Clean Architecture, DDD

---

### **5. Структура проектов, архитектурные принципы и best practices**

🔹 **Правильная организация кода в Java**

- Разбиение на пакеты (`service`, `repository`, `controller`, `config`)
- Чистая архитектура (Clean Architecture)
- Hexagonal Architecture

🔹 **Best practices в разработке Java-приложений**

- Code Style (Google Java Style Guide)
- DRY, KISS, YAGNI
- Работа с исключениями: checked vs unchecked exceptions
- Логирование и мониторинг

🔹 **Работа с зависимостями**

- Использование Maven и Gradle
- Концепция BOM (Bill of Materials)

🔹 **Версионирование и работа с Git**

- Основные команды (`clone`, `commit`, `push`, `merge`, `rebase`)
- Git Flow и Code Review

🔹 **Документирование кода**

- JavaDoc и best practices
- Swagger/OpenAPI для REST API

---

### **6. Логирование в Java и Spring Boot**

🔹 **Основы логирования**

- Log4j, Logback, JUL (Java Util Logging)
- Конфигурация логирования через `logback.xml`, `log4j.properties`
- Форматы логов: JSON, текстовый

🔹 **Логирование в Spring Boot**

- Встроенная поддержка SLF4J + Logback
- Конфигурация `application.yml`: уровни логирования (`TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`)
- Логирование HTTP-запросов (Spring Interceptor, фильтры)
- Логирование SQL-запросов Hibernate (`spring.jpa.show-sql`, `hibernate.format_sql`)

🔹 **Продвинутое логирование**

- MDC (Mapped Diagnostic Context) для отслеживания запросов
- Логирование в файлы, ротация логов
- Агрегация логов (ELK: Elasticsearch + Logstash + Kibana)
- Дистрибутивное логирование в микросервисах (Zipkin, Sleuth)

---

### **7. HTTP Servlets и работа с веб-запросами**

🔹 **Основы сервлетов**

- Что такое сервлет и зачем он нужен
- Жизненный цикл сервлета (`init()`, `service()`, `destroy()`)
- Создание простого сервлета (`HttpServlet`, `doGet()`, `doPost()`)
- Деплой сервлета в Tomcat

🔹 **Работа с запросами и ответами**

- `HttpServletRequest` и `HttpServletResponse`
- Параметры запроса (`getParameter()`, `getParameterMap()`)
- Работа с cookies и сессиями (`HttpSession`, `setAttribute()`, `getAttribute()`)

🔹 **Фильтры и слушатели (Filters & Listeners)**

- Создание и регистрация фильтров (`@WebFilter`)
- Логирование и аутентификация через фильтры
- Использование слушателей (`ServletContextListener`, `HttpSessionListener`)

🔹 **DispatcherServlet в Spring MVC**

- Как Spring использует сервлеты
- Перехват и обработка запросов

---

### **8. Работа с базами данных: JDBC**

🔹 **Основы JDBC**

- Что такое JDBC, драйверы JDBC (JDBC API, JDBC Driver, Connector)
- Подключение к базе данных через `DriverManager`
- Создание соединения (`Connection`, `Statement`, `PreparedStatement`)
- Выполнение SQL-запросов (SELECT, INSERT, UPDATE, DELETE)

🔹 **Работа с данными**

- Чтение данных (`ResultSet`)
- Использование `PreparedStatement` для защиты от SQL-инъекций
- Транзакции в JDBC (`setAutoCommit()`, `commit()`, `rollback()`)

🔹 **Оптимизация JDBC-запросов**

- Batch-обновления (`addBatch()`, `executeBatch()`)
- Connection Pooling (HikariCP, Apache DBCP)
- Работа с `DataSource` (`javax.sql.DataSource`)

🔹 **Переход от JDBC к Hibernate**

- Основные недостатки JDBC (много шаблонного кода, работа с объектами)
- Как Hibernate упрощает работу с БД

---
### **9. Hibernate и JPA**

- Основы ORM, отличие JPA от Hibernate
- Аннотации: @Entity, @Table, @Id, @GeneratedValue, @Column, @OneToMany, @ManyToOne, @ManyToMany, @JoinColumn
- Lazy vs Eager loading
- Кеширование в Hibernate (1-й и 2-й уровень)
- Работа с EntityManager и Query Language (JPQL, Criteria API)
- Миграции БД с Flyway/Liquibase

---

### **10. Spring Core**

- IoC и DI (инверсия управления и внедрение зависимостей)
- @Component, @Service, @Repository, @Controller
- Bean Lifecycle и Spring Context
- Java Config vs XML Config
- @Bean, @Configuration, @ComponentScan
- AOP (Aspect-Oriented Programming): @Aspect, @Before, @After, @Around
- EventListener и работа с событиями

---

### **11. Spring Boot**

- Создание приложения с Spring Initializr
- application.yml vs application.properties
- Профили (dev, test, prod) и @Profile
- Логирование через SLF4J + Logback
- Error Handling (@ControllerAdvice, @ExceptionHandler)
- Actuator и метрики

---

### **12. Spring MVC и REST API**

- Основы MVC (Model-View-Controller)
- Аннотации: @RestController, @RequestMapping, @GetMapping, @PostMapping, @PutMapping, @DeleteMapping
- Path и Query параметры (@RequestParam, @PathVariable)
- Валидация данных (@Valid, @NotNull, @Size, @Min, @Max)
- Exception Handling в REST API
- Jackson и работа с JSON (@JsonProperty, @JsonIgnore)
- File Upload API (multipart/form-data)

---

### **13. Spring Security**

- Основы аутентификации и авторизации
- Конфигурация SecurityFilterChain
- JWT-токены и Spring Security
- Настройка OAuth2 / OpenID Connect
- RBAC (Role-Based Access Control)

---

### **14. Spring Data и работа с БД**

- Основные аннотации: @Repository, @Transactional
- CrudRepository, JpaRepository, PagingAndSortingRepository
- Query Methods в Spring Data
- Спецификации (Specification API)
- Тонкости работы с транзакциями (@Transactional, Propagation, Isolation)

---

### **15. Кэширование**

- Встроенные механизмы Spring Cache
- Redis + Spring Cache
- CacheEvict, Cacheable, CachePut
- Оптимизация запросов в БД через кэш

---

### **16. Тестирование**

- Unit-тестирование с JUnit и Mockito
- Интеграционное тестирование с Testcontainers
- @MockBean, @WebMvcTest, @SpringBootTest
- RestAssured для тестирования API

---

### **17. Микросервисы и Spring Cloud**

- Основы микросервисной архитектуры
- API Gateway (Spring Cloud Gateway)
- Service Discovery (Eureka, Consul)
- Circuit Breaker (Resilience4j)
- Distributed Tracing (Zipkin, Sleuth)
- Config Server и управление конфигурацией

---

### **18. CI/CD и деплой**

- Сборка проекта (Maven/Gradle)
- Docker + Spring Boot
- Kubernetes (основы)
- CI/CD через GitHub Actions, GitLab CI/CD
- Логирование и мониторинг (ELK, Grafana, Prometheus)

---

### **19. Паттерны проектирования в Java**

- Singleton, Factory, Builder
- Proxy, Decorator, Adapter
- Observer, Strategy, Command
- Hexagonal Architecture, Clean Architecture, DDD

---

### **20. Архитектура и производительность**

- Архитектурные стили: Monolith, Microservices, Event-Driven
- Оптимизация запросов к БД
- Профилирование кода (JProfiler, VisualVM)
- Настройка JVM (Heap Size, GC tuning)

---

### **21. Подготовка к собеседованиям**

- Алгоритмы и структуры данных (LeetCode, Codeforces)
- Mock-интервью и разбор кейсов
- Проектное портфолио: 2–3 продвинутых проекта

---

### **Практические проекты**

1. **RESTful API для управления задачами (Task Manager)**
2. **Интернет-магазин с Spring Boot, Hibernate, Security**
3. **P2P-платежная система с микросервисами и Kafka**
4. **Приложение с WebSocket (чат в реальном времени)**
5. **Full-Stack приложение с React + Spring Boot**