
# Estudo Dirigido - Golang

Este estudo dirigido cobre tópicos fundamentais e avançados de Go (Golang), explicando conceitos, exemplos e casos de uso. Ideal para quem deseja não apenas entender, mas **dominar** a linguagem. Vamos começar!

---

## Intro. Memória, Garbage Collector, Ponteiros e Execução

- **Memória**:  
  Go gerencia memória automaticamente, usando um modelo híbrido de _stack_ e _heap_.  
  - Variáveis locais e temporárias são alocadas no _stack_, que é muito rápido e tem vida curta.  
  - Variáveis que precisam sobreviver além de uma função ou são referenciadas em outro escopo "escapam" para o _heap_.  

- **Garbage Collector (GC)**:  
  Go possui um GC moderno que identifica e remove objetos que não têm mais referências. Isso evita vazamentos de memória e permite que o desenvolvedor foque na lógica de negócios, sem preocupações manuais de desalocação.  

- **Ponteiros**:  
  Ponteiros armazenam o endereço de memória de uma variável. Eles permitem compartilhar e modificar dados sem criar cópias:  
  - `&x` obtém o endereço de `x`.  
  - `*p` acessa o valor apontado por `p`.  

```go
a := 42
p := &a
fmt.Println(*p) // 42
```

- **Execução**:  
  O runtime de Go usa um modelo chamado M:N, onde múltiplas _goroutines_ (threads leves) são multiplexadas sobre múltiplas threads do SO (_OS threads_). Isso permite escalabilidade e alto desempenho.

---

## 1. Tipos

Go é **estaticamente tipada**, ou seja, o tipo de cada variável é conhecido em tempo de compilação.  
Principais tipos:

- **Numéricos**:  
  - Inteiros: `int`, `int8`, `int16`, `int32`, `int64`.  
  - Sem sinal: `uint`, `uint8` (byte), `uint16`, `uint32`, `uint64`.  
  - Reais: `float32`, `float64`.  
  - Complexos: `complex64`, `complex128`.  

- **Boolean**: `bool` (`true` ou `false`).  

- **Strings**:  
  Cadeia imutável de bytes UTF-8.  

- **Arrays**:  
  Tamanho fixo e conhecido em tempo de compilação:  
  ```go
  var arr [3]int = [3]int{1, 2, 3}
  ```  

- **Slices**:  
  Estrutura flexível e dinâmica. São "vistas" sobre arrays e podem crescer/diminuir.  
  ```go
  s := []int{1, 2, 3}
  s = append(s, 4)
  ```  

- **Maps**:  
  Estruturas de chave-valor:  
  ```go
  m := map[string]int{"um": 1, "dois": 2}
  ```  

- **Structs**:  
  Tipos compostos para modelar entidades.

- **Interfaces**:  
  Definem comportamentos, permitindo abstração e polimorfismo.

---

## 2. Fluxos

Controle de fluxo em Go:

- **For**: Única estrutura de repetição.
```go
for i := 0; i < 10; i++ {
    fmt.Println(i)
}
```

- **While**: Emulado com `for`.
```go
for x < 10 {
    fmt.Println(x)
    x++
}
```

- **If/Else**:
```go
if cond {
    // ...
} else {
    // ...
}
```

- **Switch**: Elegante para múltiplos casos.
```go
switch dia {
case "segunda":
    fmt.Println("Início da semana")
case "sexta":
    fmt.Println("Fim da semana")
default:
    fmt.Println("Dia comum")
}
```

---

## 3. Funções

- Definição básica:
```go
func soma(a int, b int) int {
    return a + b
}
```

- Retorno múltiplo:
```go
func divide(a, b int) (int, int) {
    return a / b, a % b
}
```

- Variadic:
```go
func somaTudo(valores ...int) int {
    total := 0
    for _, v := range valores {
        total += v
    }
    return total
}
```

- Funções anônimas:
```go
f := func(x int) int { return x * x }
```

- **Funções são de primeira classe**: podem ser atribuídas a variáveis, passadas como parâmetros e retornadas.

---

## 4. Structs

São usadas para modelar dados:

```go
type Pessoa struct {
    Nome string
    Idade int
}
```

- Instanciação:
```go
p := Pessoa{"Ana", 30}
```

- Métodos:
```go
func (p Pessoa) Saudacao() {
    fmt.Println("Olá,", p.Nome)
}
```
> Nota: Recebedores por valor fazem cópia; por ponteiro (`*Pessoa`) modificam o original.

---

## 5. Interfaces

Interfaces permitem abstrair comportamentos.  
Exemplo:
```go
type Animal interface {
    Falar() string
}
```

Implementação:
```go
type Cachorro struct{}
func (c Cachorro) Falar() string {
    return "Au Au!"
}
```

Interfaces são satisfeitas **implicitamente**. Se um tipo implementa todos os métodos, ele automaticamente "satisfaz" a interface.

---

## 6. Defer

`defer` agenda uma instrução para o final da função, útil para liberar recursos:

```go
func exemplo() {
    defer fmt.Println("Fim!")
    fmt.Println("Durante execução")
}
```

Execução do `defer` é em ordem LIFO (último a entrar, primeiro a sair).

---

## 7. I/O

- **Leitura**:
```go
data, err := ioutil.ReadFile("arquivo.txt")
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(data))
```

- **Escrita**:
```go
err := ioutil.WriteFile("saida.txt", []byte("Conteúdo"), 0644)
```

---

## 8. Concorrência

Concorrência em Go é baseada no modelo CSP (_Communicating Sequential Processes_).  
Em vez de _threads_ complexas, usamos:

- **Goroutines**: Execução concorrente.  
- **Channels**: Comunicação segura entre goroutines.

---

## 9. Goroutines

Goroutines são funções leves e concorrentes, iniciadas com a palavra-chave `go`:

```go
go func() {
    fmt.Println("Executando concorrente")
}()
```

- **Leves**: Consomem poucos KBs.  
- **Escalonadas pelo runtime de Go**: Múltiplas goroutines podem rodar sobre um único thread do SO, e o runtime as gerencia.  
- **Independentes**: Se uma goroutine _panicar_, outras continuam.

---

## 10. Channels

Channels são **túneis de comunicação** entre goroutines.  
Permitem **sincronização** e **troca de dados** de forma segura.

- Criação:
```go
ch := make(chan int) // channel de int não-bufferizado
```

- Envio e recebimento:
```go
ch <- 42    // envia 42
x := <- ch  // recebe 42
```

- **Buffered channels**:
```go
ch := make(chan int, 2)
ch <- 1
ch <- 2
```
> Permitem enviar até N itens sem bloqueio.

- **Select**: Multiplexa múltiplos canais.
```go
select {
case v := <- ch1:
    fmt.Println(v)
case ch2 <- 42:
    fmt.Println("Enviado para ch2")
default:
    fmt.Println("Nenhum pronto")
}
```

---

## 11. Mutex

Em casos onde múltiplas goroutines acessam e **modificam recursos compartilhados**, usamos `sync.Mutex` para evitar **race conditions**.

```go
var mu sync.Mutex
contador := 0

func incrementar() {
    mu.Lock()
    contador++
    mu.Unlock()
}
```

- **Lock()**: Bloqueia acesso ao recurso.  
- **Unlock()**: Libera para outras goroutines.  
- **Boa prática**: Use `defer mu.Unlock()` logo após `mu.Lock()` para garantir liberação.

---

## Conclusão

Esses conceitos são essenciais para **desenvolvimento robusto e concorrente em Go**.  
Recomenda-se **praticar e testar** cada ponto. Caso queira exemplos adicionais ou exercícios, posso ajudar! 🚀

---

**Autor**: Victor Reboredo  
**Contato**: victoreboredo.dev@gmail.com

---

🎯 **Objetivo**: Tornar o aprendizado mais fluido e prático. Bons estudos!
