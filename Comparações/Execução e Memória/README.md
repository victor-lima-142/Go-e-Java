
# 📚 Estudo Dirigido: Concorrência, Paralelismo, Gerenciamento de Memória e Performance

Este guia oferece um estudo completo e prático sobre dois temas essenciais em **Java** e **Go**: **Concorrência e Paralelismo** e **Gerenciamento de Memória e Performance**. Vamos tornar isso mais dinâmico com exemplos práticos e snippets de código prontos para uso!

---

## 🚀 1. Concorrência e Paralelismo

### Conceitos Fundamentais

- **Concorrência**: Capacidade de lidar com várias tarefas de forma independente (não necessariamente simultânea).
- **Paralelismo**: Execução de múltiplas tarefas ao mesmo tempo, aproveitando múltiplos núcleos ou threads.

🔑 Esses conceitos são vitais para criar aplicações escaláveis e performáticas!

### ⚡ Go: Goroutines, Channels e o pacote `sync`

✅ **Goroutines**: Threads leves e baratas em memória.

```go
go func() {
    fmt.Println("Executando goroutine!")
}()
```

✅ **Channels**: Comunicação segura entre goroutines, evitando condições de corrida.

```go
jobs := make(chan int, 10)
results := make(chan int, 10)

go func() {
    for j := range jobs {
        results <- j * 2
    }
}()
```

✅ **Pacote sync**: Fornece primitivas como `sync.Mutex` e `sync.WaitGroup`.

```go
var mu sync.Mutex
mu.Lock()
// operação crítica
mu.Unlock()
```

### ⚡ Java: Threads, Executors e CompletableFuture

✅ **Threads**: Criação manual de threads.

```java
Thread t = new Thread(() -> System.out.println("Executando na thread!"));
t.start();
```

✅ **Executors**: Gerenciamento de pools de threads.

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
executor.submit(() -> System.out.println("Rodando no Executor!"));
executor.shutdown();
```

✅ **CompletableFuture**: Programação assíncrona e pipelines.

```java
CompletableFuture.supplyAsync(() -> "Resultado!")
    .thenAccept(result -> System.out.println("Recebido: " + result));
```

### 🔎 Comparação Rápida

| Conceito        | Go                       | Java                                    |
|------------------|-------------------------|-----------------------------------------|
| Modelo leve     | Goroutines               | Threads                                  |
| Comunicação     | Channels                 | Shared memory (locks, synchronized)     |
| Abstrações      | sync (Mutex, WaitGroup)  | Executors, CompletableFuture             |
| Ideal para      | Concorrência leve e I/O  | Carga pesada e integração corporativa    |

⚠️ **Problemas Comuns**: deadlocks, race conditions e starvation.

---

## 🧠 2. Gerenciamento de Memória e Performance

### 🗑️ Garbage Collector

✅ **Java**: GC da JVM (G1, ZGC), pausas periódicas, ajuste por flags como `-Xmx` e `-Xms`.

✅ **Go**: GC concurrent e generational com baixa latência.

### 🔧 Ferramentas de Profiling

✅ **Java**:
- **VisualVM** e **Java Mission Control** para análise de heap e CPU.
- Inicie sua app: `java -jar MinhaApp.jar`.
- Conecte no VisualVM e monitore!

✅ **Go**:
- **pprof** embutido para análise de heap, CPU e goroutines.

```go
import _ "net/http/pprof"
go func() {
    log.Println(http.ListenAndServe("localhost:6060", nil))
}()
```

Execute no terminal:

```bash
go tool pprof http://localhost:6060/debug/pprof/profile
```

### 🌟 Boas Práticas

✅ **Java**:  
- Use tipos primitivos sempre que possível.  
- Evite criar objetos temporários.  
- Monitore o uso de heap!  

✅ **Go**:  
- Reuse slices e structs.  
- Utilize `sync.Pool` para objetos temporários e reduzir GC overhead.  

---

## 🎯 Resumo

- **Go**: Leve, ótimo para serviços I/O-bound e concorrência.  
- **Java**: Robusto, ideal para aplicações corporativas e complexas.  
- Ferramentas de profiling e práticas de sincronização são essenciais para desempenho máximo!

---

**Autor**: Victor Reboredo  
**Contato**: victoreboredo.dev@gmail.com

---

🎯 **Objetivo**: Tornar o aprendizado mais fluido e prático. Bons estudos!
