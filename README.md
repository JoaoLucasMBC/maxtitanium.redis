# **maxtitanium.redis**

`by: João Lucas Cadorniga`

Configurações do redis para a AWS podem ser encontradas na pasta `/k8s`

Todas as outras configurações se encontram no arquivo `docker-compose.yaml` do repo [maxtitanium.docker-api](https://github.com/JoaoLucasMBC/maxtitanium.docker-api). Também pode ser visualizado abaixo:

```yaml
redis:
    container_name: redis
    image: redis:latest
    ports:
      - 6379:6379
    restart: always
    networks:
      - private-network
```

## Utilização

A partir disso, os resources do serviço em Spring Boot utilizam o Redis para armazenar os dados. Para isso, é necessário configurar o `application.yaml` do projeto Spring Boot. Abaixo, segue um exemplo de configuração:

```yaml
spring:
  redis:
    host: redis
    port: 6379
```

No arquivo de inicialização:

```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableCaching
@EnableFeignClients(basePackages = "max.product")
public class OrderApplication {

	public static void main(String[] args) {
		SpringApplication.run(OrderApplication.class, args);
	}
}
```

No arquivo de serviço:

```java
@CachePut(value = "orders", key = "#result.id()")
    public Order create(Order in) {
```

ou

```java
@Cacheable(value = "orders", key = "#id")
    public Order findById(String id) {
```