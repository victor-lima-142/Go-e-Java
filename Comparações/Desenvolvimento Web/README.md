
# Estudo Dirigido: Java e Go para Desenvolvimento Web e Microsserviços

Este documento fornece um guia abrangente e prático para dominar frameworks, testes, boas práticas e implantação de microsserviços utilizando **Java** e **Go**. Além de exemplos concretos, inclui conceitos avançados para impulsionar sua carreira no desenvolvimento de APIs RESTful e microsserviços.

---

## 📦 1. Frameworks e Bibliotecas Essenciais

### 🔷 Java: Spring Boot
- **Principais frameworks e bibliotecas**:
  - Spring Boot (auto-configuração e starters)
  - Spring Data JPA (acesso a dados com ORM)
  - Hibernate (mapeamento objeto-relacional)
  - Spring Security (segurança e autenticação)

- **Exemplo de Modelo `Produto`**:
  ```java
  @Entity
  public class Produto {
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;

      private String nome;
      private Double preco;

      // getters e setters
  }
  ```

- **Exemplo de Repositório**:
  ```java
  public interface ProdutoRepository extends JpaRepository<Produto, Long> {
  }
  ```

- **Exemplo de Controlador REST**:
  ```java
  @RestController
  @RequestMapping("/api/produtos")
  public class ProdutoController {

      @Autowired
      private ProdutoRepository produtoRepository;

      @GetMapping
      public List<Produto> listar() {
          return produtoRepository.findAll();
      }

      @PostMapping
      public Produto salvar(@RequestBody Produto produto) {
          return produtoRepository.save(produto);
      }
  }
  ```

- **Exemplo de Middleware (Filter)**:
  ```java
  @Component
  public class LoggingFilter implements Filter {
      @Override
      public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
              throws IOException, ServletException {
          System.out.println("Requisição recebida: " + ((HttpServletRequest) request).getRequestURI());
          chain.doFilter(request, response);
      }
  }
  ```

---

### 🟠 Go: Gin Framework
- **Principais frameworks e bibliotecas**:
  - Gin (roteamento rápido e eficiente)
  - Gorm (ORM para bancos relacionais)
  - Swagger (documentação interativa de APIs)
  - Wire (injeção de dependências)

- **Exemplo de Modelo `Produto`**:
  ```go
  type Produto struct {
      ID    uint    `json:"id" gorm:"primaryKey"`
      Nome  string  `json:"nome"`
      Preco float64 `json:"preco"`
  }
  ```

- **Exemplo de Conexão e Rotas**:
  ```go
  func main() {
      r := gin.Default()
      db, _ := gorm.Open(sqlite.Open("produtos.db"), &gorm.Config{})
      db.AutoMigrate(&Produto{})

      r.GET("/api/produtos", func(c *gin.Context) {
          var produtos []Produto
          db.Find(&produtos)
          c.JSON(200, produtos)
      })

      r.POST("/api/produtos", func(c *gin.Context) {
          var produto Produto
          if err := c.ShouldBindJSON(&produto); err == nil {
              db.Create(&produto)
              c.JSON(201, produto)
          } else {
              c.JSON(400, gin.H{"error": err.Error()})
          }
      })

      r.Use(LoggerMiddleware)
      r.Run(":8080")
  }
  ```

- **Exemplo de Middleware em Go**:
  ```go
  func LoggerMiddleware(c *gin.Context) {
      fmt.Println("Requisição recebida:", c.Request.RequestURI)
      c.Next()
  }
  ```

---

## 🧪 2. Testes e Boas Práticas

### 🔷 Java: JUnit e Spring Boot Test
- **Exemplo de Teste Unitário**:
  ```java
  @WebMvcTest(ProdutoController.class)
  public class ProdutoControllerTest {

      @Autowired
      private MockMvc mockMvc;

      @MockBean
      private ProdutoRepository produtoRepository;

      @Test
      public void deveRetornarListaDeProdutos() throws Exception {
          mockMvc.perform(get("/api/produtos"))
                 .andExpect(status().isOk());
      }
  }
  ```

- **Boas práticas Java**:
  - Clean Code: nomes claros, métodos pequenos
  - SOLID: princípios fundamentais de design
  - Padrões de projeto: Factory, Repository, Singleton

### 🟠 Go: Pacote `testing` e httptest
- **Exemplo de Teste Unitário Simples**:
  ```go
  func TestSoma(t *testing.T) {
      resultado := 2 + 2
      if resultado != 4 {
          t.Errorf("Esperava 4, mas obteve %d", resultado)
      }
  }
  ```

- **Exemplo de Teste de Integração com httptest**:
  ```go
  func TestGetProdutos(t *testing.T) {
      router := setupRouter()
      w := httptest.NewRecorder()
      req, _ := http.NewRequest("GET", "/api/produtos", nil)
      router.ServeHTTP(w, req)

      if w.Code != http.StatusOK {
          t.Errorf("Esperava status 200, mas obteve %d", w.Code)
      }
  }
  ```

---

## ☁️ 3. Microsserviços e Cloud

### 🔷 Java: Docker e Kubernetes
- **Exemplo de Dockerfile**:
  ```dockerfile
  FROM openjdk:17-jdk-alpine
  ARG JAR_FILE=target/*.jar
  COPY ${JAR_FILE} app.jar
  ENTRYPOINT ["java", "-jar", "/app.jar"]
  ```

- **Exemplo de Deployment Kubernetes**:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: produto-service
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: produto
    template:
      metadata:
        labels:
          app: produto
      spec:
        containers:
        - name: produto-container
          image: produto:latest
          ports:
          - containerPort: 8080
  ```

### 🟠 Go: Docker e Kubernetes
- **Exemplo de Dockerfile**:
  ```dockerfile
  FROM golang:1.21-alpine as builder
  WORKDIR /app
  COPY . .
  RUN go build -o main .

  FROM alpine:latest
  WORKDIR /root/
  COPY --from=builder /app/main .
  CMD ["./main"]
  ```

- **Exemplo de Deployment Kubernetes**:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: go-produto-service
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: go-produto
    template:
      metadata:
        labels:
          app: go-produto
      spec:
        containers:
        - name: go-produto-container
          image: go-produto:latest
          ports:
          - containerPort: 8080
  ```

---

## 🎯 Recomendações Finais

✅ Desenvolva APIs RESTful reais para fixar o aprendizado  
✅ Estude profundamente os princípios SOLID e Clean Code  
✅ Implemente seus microsserviços com Docker e Kubernetes  
✅ Pratique diariamente para evolução contínua 🚀  

---

**Autor**: Victor Reboredo  
**Contato**: victoreboredo.dev@gmail.com

---

🎯 **Objetivo**: Tornar o aprendizado mais fluido e prático. Bons estudos!