
# 📝 Estudo Dirigido: Java vs Go

Este README é um guia completo para comparação entre Java e Go, incluindo exemplos de código, dicas práticas e observações sobre paradigmas.

---

## 📌 Índice

1. [Fundamentos e Sintaxe](#fundamentos-e-sintaxe)
2. [POO em Java vs Organização Concisa em Go](#poo-em-java-vs-organização-concisa-em-go)
3. [Comparação Detalhada](#comparação-detalhada)
4. [Dicas Finais](#dicas-finais)
5. [Exemplos Práticos](#exemplos-práticos)

---

## 🧩 Fundamentos e Sintaxe

### ✅ Java

#### 🔹 Estruturas de Controle
```java
if (x > 0) {
    System.out.println("x é positivo");
} else {
    System.out.println("x é negativo ou zero");
}

for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

#### 🔹 Funções/Métodos
```java
public int soma(int a, int b) {
    return a + b;
}
```

#### 🔹 Tipos de Dados
- **Primitivos**: `int`, `float`, `double`, `char`, `boolean`
- **Referências**: classes, interfaces, arrays

#### 🔹 Manipulação de Variáveis
```java
int numero = 10;
final double PI = 3.14;
```

#### 🔹 Observações
- **Orientado a Objetos** puro.
- Usa palavras-chave como `final`, `static` e modificadores de acesso.

---

### ✅ Go (Golang)

#### 🔹 Estruturas de Controle
```go
if x > 0 {
    fmt.Println("x é positivo")
} else {
    fmt.Println("x é negativo ou zero")
}

for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

#### 🔹 Funções
```go
func soma(a int, b int) int {
    return a + b
}
```

#### 🔹 Tipos de Dados
- Básicos: `int`, `float64`, `string`, `bool`
- **Zero value** por padrão (`0`, `0.0`, `false`, `""`)

#### 🔹 Manipulação de Variáveis
```go
x := 10
```

#### 🔹 Observações
- **Inferência de tipos** (`:=`).
- Exportação pela primeira letra maiúscula.

---

### 🔎 Comparação Java vs Go

| Aspecto        | Java                              | Go                                  |
|----------------|-----------------------------------|-------------------------------------|
| Paradigma      | Orientado a Objetos tradicional   | Estrutural e conciso                |
| Laços          | `for`, `while`, `do-while`       | Apenas `for`                        |
| Declaração     | `tipo variável = valor`          | `var` ou `:=`                       |
| Funções        | Métodos em classes                | Funções de primeira classe          |
| Tipagem        | Primitivos e referências          | Zero value                          |
| Visibilidade   | Modificadores de acesso           | Exportação pela letra maiúscula     |

---

## 🏛️ POO em Java vs Organização Concisa em Go

### ☕ Java: Pilares da POO

- **Encapsulamento**: proteção via modificadores.
- **Herança**: reuso de código (ex.: `extends`).
- **Polimorfismo**: várias implementações.
- **Abstração**: classes e métodos abstratos.

#### 🔹 Exemplo:
```java
public class Conta {
    private double saldo;

    public double getSaldo() { return saldo; }

    public void depositar(double valor) {
        saldo += valor;
    }
}
```

---

### 🟨 Go: Estruturas de Código

- Sem classes, usa **structs** e **interfaces**.
- **Composição** em vez de herança.
- Interfaces são implementadas implicitamente.

#### 🔹 Exemplo:
```go
type Animal interface {
    Comer()
}

type Cachorro struct{}

func (c Cachorro) Comer() {
    fmt.Println("Comendo...")
}
```

---

### 🔎 Comparação de POO

| Aspecto       | Java                          | Go                              |
|---------------|-------------------------------|----------------------------------|
| Herança       | Suporte direto (`extends`)    | Composição de structs           |
| Encapsulamento| Modificadores de acesso       | Exportação pela inicial maiúscula|
| Polimorfismo  | Interfaces e classes abstratas| Interfaces implícitas           |
| Abstração     | Classes e métodos abstratos   | Interfaces e composição         |

---

## 💡 Dicas Finais

✅ **Em Java**:
- Pratique hierarquias de classes e uso de exceções.
- Explore bibliotecas como Spring, Hibernate e JPA.

✅ **Em Go**:
- Use composição de structs e interfaces.
- Estude pacotes como `net/http` para criar APIs.
- Explore **concorrência** com goroutines e channels.

✅ **Para ambos**:
- Crie APIs REST reais para consolidar o aprendizado.
- Compare abordagens e refatore seus projetos!

---

📚 **Conclusão**: O importante não é só aprender a sintaxe, mas também praticar boas práticas e entender como cada linguagem resolve problemas reais.

---

**Autor**: Victor Reboredo  
**Contato**: victoreboredo.dev@gmail.com

---

🎯 **Objetivo**: Tornar o aprendizado mais fluido e prático. Bons estudos!
