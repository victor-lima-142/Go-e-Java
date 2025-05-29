
# 📚 Estudo Dirigido: Java x Go

## 1️⃣ Frameworks Java: Spring Boot, Quarkus e Micronaut

### 🚀 Ciclo de Vida de um Request no **Spring Boot**
1️⃣ **Recepção do Request**  
O **DispatcherServlet** atua como Front Controller e direciona a requisição para o handler correto.

2️⃣ **Filtros (Filters)**  
São implementados via `javax.servlet.Filter`.  
Usados para logging, CORS, autenticação etc.  
**Exemplo:**
```java
@Component
public class LoggingFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Request URI: " + ((HttpServletRequest) request).getRequestURI());
        chain.doFilter(request, response);
    }
}
```

3️⃣ **Interceptors**  
Usam `HandlerInterceptor`, que permite executar lógica antes ou depois do método do controller.  
**Exemplo:**
```java
@Component
public class HeaderValidatorInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        if (request.getHeader("X-API-KEY") == null) {
            response.setStatus(HttpServletResponse.SC_FORBIDDEN);
            return false;
        }
        return true;
    }
}
```

4️⃣ **AOP (Aspect-Oriented Programming)**  
Aspectos transversais com `@Aspect` e `@Around`, via AspectJ ou Spring AOP.  
**Exemplo:**
```java
@Aspect
@Component
public class LoggingAspect {
    @Around("execution(* com.example.service.*.*(..))")
    public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("Before: " + joinPoint.getSignature());
        Object result = joinPoint.proceed();
        System.out.println("After: " + joinPoint.getSignature());
        return result;
    }
}
```

5️⃣ **Handler Mapping e Handler Adapter**  
Encontram o método do controller correto e o executam.

6️⃣ **View Resolver**  
Renderiza a view ou responde JSON, utilizando `HttpMessageConverter` para serialização de dados.

7️⃣ **Camada de Serviço e Repositório**  
Lógica de negócio e persistência de dados (JPA, JDBC, etc.).  
**Exemplo Repositório JPA:**
```java
public interface UserRepository extends JpaRepository<User, Long> {}
```

### ⚡️ Comparação com **Quarkus** e **Micronaut**

| Aspecto                   | Spring Boot                  | Quarkus                        | Micronaut                       |
|---------------------------|------------------------------|--------------------------------|---------------------------------|
| Injeção de Dependência    | Reflection (Spring DI)       | Build-time (GraalVM)          | Compile-time (AOT)              |
| Startup Time              | Mais lento em cloud-native   | Muito rápido (nativo)         | Rápido (AOT, sem reflection)    |
| AOP                       | AspectJ/Spring AOP           | Arc AOP                        | Micronaut AOP (AOT)             |
| DispatcherServlet         | Usa DispatcherServlet        | Sem Servlet em nativo         | Sem Servlet tradicional         |
| Reflection                | Amplamente usado             | Evita reflection              | Evita reflection (AOT)          |

**Exemplo Quarkus**:  
```java
@Path("/hello")
public class HelloResource {
    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "Hello from Quarkus!";
    }
}
```

**Exemplo Micronaut**:  
```java
@Controller("/hello")
public class HelloController {
    @Get("/")
    public String index() {
        return "Hello from Micronaut!";
    }
}
```

### 🔧 Otimizações para Alta Carga
✅ **Connection Pooling**: HikariCP configurado via YAML.  
✅ **Cache de Anotações**: @Cacheable com Ehcache ou Caffeine.  
✅ **Thread Pool**: ajuste de Tomcat/Undertow para alta concorrência.  
✅ **Native Image**: Quarkus/Micronaut com GraalVM.  
✅ **Observabilidade**: Micrometer, OpenTelemetry.

---

## 2️⃣ Middlewares, Filters e Guards

### 🔥 Java - Spring Boot Filters/Interceptors
✅ **Logging**:  
```java
@Component
public class LoggingFilter implements Filter { ... }
```

✅ **Validação de Headers**:  
```java
@Component
public class HeaderInterceptor implements HandlerInterceptor { ... }
```

✅ **Circuit Breaker e Rate Limiting**:  
Com **Spring Cloud Gateway** e **Resilience4j**:
```yaml
spring.cloud.gateway.routes:
  - id: myService
    uri: http://my-service
    filters:
      - name: RequestRateLimiter
        args:
          redis-rate-limiter.replenishRate: 10
          redis-rate-limiter.burstCapacity: 20
```

---

### 🐹 Go - Middlewares com **Gin** e **Echo**

✅ **Logging**:
```go
func Logger() gin.HandlerFunc {
    return func(c *gin.Context) {
        fmt.Println("Request:", c.Request.URL.Path)
        c.Next()
    }
}
```

✅ **Validação de Headers**:
```go
func APIKeyMiddleware() gin.HandlerFunc {
    return func(c *gin.Context) {
        if c.GetHeader("X-API-KEY") == "" {
            c.AbortWithStatus(http.StatusForbidden)
            return
        }
        c.Next()
    }
}
```

✅ **Circuit Breaker**:  
Utilizando **sony/gobreaker**:
```go
cb := gobreaker.NewCircuitBreaker(gobreaker.Settings{Name: "MyCB"})
result, err := cb.Execute(func() (interface{}, error) {
    return http.Get("https://api.example.com")
})
```

✅ **Rate Limiting**:  
```go
limiter := rate.NewLimiter(1, 5) // 1 req/s com burst de 5
if !limiter.Allow() {
    c.AbortWithStatus(http.StatusTooManyRequests)
}
```

### ⚖️ Comparação

| Recurso               | Spring Cloud Gateway             | Go (Gin/Echo Middleware)  |
|-----------------------|----------------------------------|---------------------------|
| Configuração          | YAML e programação               | Imperativo com handler    |
| Rate Limiting         | Redis RateLimiter integrado      | ulule/limiter ou custom   |
| Circuit Breaker       | Resilience4j                     | sony/gobreaker            |
| Validação de Headers  | Global Filter/Interceptor        | Middleware direto         |

---

## 3️⃣ Autenticação e Autorização Avançada

### 🔑 OAuth2 + JWT em **Spring Boot**
✅ **Configuração do Resource Server**:
```yaml
spring.security.oauth2.resourceserver.jwt.issuer-uri: https://keycloak.example.com/realms/myrealm
```

✅ **Autorização baseada em Scopes/RBAC**:
```java
@PreAuthorize("hasAuthority('SCOPE_read')")
@GetMapping("/secure-data")
public String getSecureData() { ... }
```

✅ **Integração com Keycloak/Auth0**:  
Configuração automática via `issuer-uri`.

✅ **Replay Mitigation**:  
- Uso de **JTI** para detectar replays.  
- **Refresh tokens** armazenados em banco/Redis com expiração curta.

---

### 🐹 OAuth2 + JWT em **Go**
✅ **Usando golang-jwt**:
```go
token, _ := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
    return []byte("mysecret"), nil
})
if claims, ok := token.Claims.(jwt.MapClaims); ok && token.Valid {
    fmt.Println("User:", claims["sub"])
}
```

✅ **Integração com Keycloak** usando **github.com/MicahParks/keyfunc**:
```go
jwks, err := keyfunc.Get("https://keycloak.example.com/realms/myrealm/protocol/openid-connect/certs", keyfunc.Options{})
```

✅ **Armazenamento Seguro**:  
- JWTs são stateless e não armazenados no servidor.  
- Refresh tokens guardados no Redis para maior segurança.

---

🎯 **Conclusão**
Este estudo dirigido cobre:

✅ Ciclo de vida em frameworks Java (Spring, Quarkus, Micronaut)  
✅ Middlewares e comparações práticas em Go (Gin/Echo) e Java  
✅ Implementação avançada de OAuth2/JWT com Keycloak/Auth0  
✅ Estratégias de mitigação de vulnerabilidades e otimização para alta carga.

---

**Autor**: Victor Reboredo  
**Contato**: victoreboredo.dev@gmail.com

---

🎯 **Objetivo**: Tornar o aprendizado mais fluido e prático. Bons estudos!