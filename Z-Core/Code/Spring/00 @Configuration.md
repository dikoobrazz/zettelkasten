2024110422:47
___
Date: 04-11-2024 | 22:47
Tags: #spring #annotation #configuration
mapofcontents: [[zero-links|OO Links]]
___
## @Configuration

Аннотация **@Configuration** указывает Spring, что это класс конфигурации, который предоставит bean-компоненты контексту приложения Spring. Методы конфигурации аннотируются @Bean, указывая, что объекты, которые они возвращают, должны быть добавлены как **bean**-компоненты в контекст приложения (где, по умолчанию, их соответствующие идентификаторы **bean**-компонентов будут такими же, как имена методов, которые их определяют).

```java
@Configuration
public class ServiceConfiguration {
	@Bean
	public InventoryService inventoryService() {
		return new InventoryService();
	}
	@Bean
	public ProductService productService() {
		return new ProductService(inventoryService());
	}
}
```


-----
**Zero-Links**  (Внутренние ссылки) Линки - ключевые слова
-

------
**Links** (Внешние ссылки)
-
