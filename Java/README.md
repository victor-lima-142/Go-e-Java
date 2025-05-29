
# Estudo Dirigido Extensivo sobre Java

## 1. Historicidade e Uso

A linguagem de programação **Java** foi desenvolvida no início dos anos 1990 por James Gosling, Mike Sheridan e Patrick Naughton, engenheiros da Sun Microsystems. Inicialmente, o projeto era denominado **Oak**, cujo objetivo era fornecer uma plataforma para dispositivos embarcados e eletrodomésticos inteligentes. No entanto, com o crescimento da Internet e a necessidade de aplicações portáveis e seguras, Java foi reorientado para se tornar uma linguagem de propósito geral voltada à criação de aplicações para a web.

O lançamento oficial do Java ocorreu em 1995, com a promessa de “**Write Once, Run Anywhere**” (escreva uma vez, rode em qualquer lugar). Essa abordagem visava eliminar a complexidade associada ao desenvolvimento de aplicações portáveis, oferecendo uma Máquina Virtual Java (**JVM**) que interpretava bytecode Java em qualquer sistema operacional compatível.

Inicialmente, Java foi muito utilizado na criação de **applets**, pequenos programas executados em navegadores, o que demonstrava sua capacidade de rodar em ambientes heterogêneos de forma segura e eficiente. Essa característica impulsionou sua popularidade e fomentou a criação de uma ampla gama de aplicações corporativas, móveis e embarcadas.

Exemplo clássico de um programa Java que imprime “Hello, World!”:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Este exemplo demonstra a simplicidade da sintaxe Java e o uso de sua estrutura de classes e métodos.

---

## 2. Tipos e Variáveis

Java é uma linguagem **estaticamente tipada**, o que significa que as variáveis devem ter seu tipo definido no momento de sua declaração. Os **tipos primitivos** em Java são fundamentais para o armazenamento eficiente de dados simples:

- `byte` (8 bits)
- `short` (16 bits)
- `int` (32 bits)
- `long` (64 bits)
- `float` (32 bits - ponto flutuante)
- `double` (64 bits - ponto flutuante)
- `boolean` (verdadeiro/falso)
- `char` (caractere Unicode)

Além dos tipos primitivos, Java possui classes **wrapper** que encapsulam esses tipos em objetos (`Integer`, `Double`, etc.), permitindo maior flexibilidade ao trabalhar com APIs e coleções.

### Escopo e Modificadores

As variáveis possuem escopos distintos:
- **Variáveis locais:** declaradas dentro de métodos e destruídas ao término do bloco.
- **Variáveis de instância:** associadas a cada objeto.
- **Variáveis de classe (static):** associadas à classe como um todo.

Exemplo demonstrativo:

```java
public class Variaveis {
    static int contagemGlobal = 0; // variável de classe

    public static void main(String[] args) {
        final int constante = 100; // constante (não pode ser alterada)
        String mensagem = "Variáveis em Java"; // variável local
        if (mensagem != null) {
            String status = "Ativo";
            System.out.println(status);
        }
        // System.out.println(status); // erro: fora de escopo
    }
}
```

---

## 3. Arrays e ArrayList

### Arrays

Arrays são estruturas homogêneas de tamanho fixo, ideais para armazenar coleções simples.

```java
int[] numeros = {1, 2, 3, 4, 5};
for (int numero : numeros) {
    System.out.println(numero);
}
```

Arrays podem ser multidimensionais:

```java
int[][] matriz = {{1, 2}, {3, 4}};
System.out.println(matriz[0][1]); // 2
```

### ArrayList

A classe `ArrayList` pertence ao pacote `java.util` e implementa a interface `List`. Ela permite listas dinâmicas e facilita a inserção, remoção e acesso aos elementos.

```java
import java.util.ArrayList;
import java.util.List;

List<String> nomes = new ArrayList<>();
nomes.add("Ana");
nomes.add("Carlos");
System.out.println(nomes);
```

---

## 4. Estruturas de Dados

### Listas (LinkedList)

A estrutura de lista encadeada permite inserções e remoções eficientes:

```java
import java.util.LinkedList;
LinkedList<String> listaEncadeada = new LinkedList<>();
listaEncadeada.add("A");
listaEncadeada.add("B");
System.out.println(listaEncadeada);
```

### Pilhas (Stacks)

Estruturas **LIFO** (Last In, First Out):

```java
import java.util.Stack;
Stack<Integer> pilha = new Stack<>();
pilha.push(10);
pilha.push(20);
System.out.println(pilha.pop()); // remove 20
```

### Filas (Queues)

Estruturas **FIFO** (First In, First Out):

```java
import java.util.LinkedList;
import java.util.Queue;

Queue<String> fila = new LinkedList<>();
fila.offer("Elemento 1");
fila.offer("Elemento 2");
System.out.println(fila.poll()); // Elemento 1
```

### Árvores

Exemplo básico de uma árvore binária:

```java
class No {
    int valor;
    No esquerda, direita;

    public No(int valor) {
        this.valor = valor;
    }
}
```

### Grafos

Estruturas que modelam relações complexas:

```java
import java.util.*;

class Grafo {
    private Map<Integer, List<Integer>> adjacencias = new HashMap<>();

    public void adicionarAresta(int u, int v) {
        adjacencias.putIfAbsent(u, new ArrayList<>());
        adjacencias.get(u).add(v);
    }
}
```

---

### Map, HashMap e Hashtable

Essas coleções armazenam pares **chave-valor**. `HashMap` é mais eficiente e não sincronizado, enquanto `Hashtable` é sincronizado e mais seguro para ambientes multi-thread.

```java
import java.util.HashMap;
import java.util.Hashtable;
import java.util.Map;

Map<String, Integer> mapa = new HashMap<>();
mapa.put("A", 1);
System.out.println(mapa.get("A"));

Map<String, Integer> tabela = new Hashtable<>();
tabela.put("B", 2);
System.out.println(tabela.get("B"));
```

---

## 6. Orientação a Objetos

A POO visa representar conceitos do mundo real em código. Seus pilares são:

1. **Abstração**: foco nos aspectos essenciais.
2. **Encapsulamento**: oculta detalhes internos e expõe apenas o necessário.
3. **Herança**: permite a criação de hierarquias.
4. **Polimorfismo**: permite o mesmo método se comportar de formas diferentes.

```java
class Animal {
    String nome;
    void emitirSom() {
        System.out.println("Som genérico");
    }
}
```

### Princípios SOLID

- **S**: Single Responsibility (uma única responsabilidade)
- **O**: Open/Closed (aberto para extensão, fechado para modificação)
- **L**: Liskov Substitution (substituição por subclasses)
- **I**: Interface Segregation (interfaces coesas)
- **D**: Dependency Inversion (inversão de dependências)

---

## 7. Classes

As classes são as unidades fundamentais:

```java
public class Pessoa {
    private String nome;
    public Pessoa(String nome) {
        this.nome = nome;
    }
    public void exibirNome() {
        System.out.println(nome);
    }
}
```

---

## 8. Polimorfismo

```java
class Cachorro extends Animal {
    void emitirSom() {
        System.out.println("Au Au!");
    }
}
```

---

## 9. Interfaces

Interfaces definem contratos que as classes devem implementar.

```java
interface Veiculo {
    void mover();
}

class Carro implements Veiculo {
    public void mover() {
        System.out.println("Carro está em movimento.");
    }
}
```

---

## 10. Decorators

Permitem adicionar funcionalidades a objetos de forma dinâmica:

```java
interface Notificador {
    void enviar(String mensagem);
}

class EmailNotificador implements Notificador {
    public void enviar(String mensagem) {
        System.out.println("Enviando e-mail: " + mensagem);
    }
}

class DecoradorNotificador implements Notificador {
    private Notificador wrappee;
    public DecoradorNotificador(Notificador wrappee) {
        this.wrappee = wrappee;
    }
    public void enviar(String mensagem) {
        wrappee.enviar(mensagem);
        System.out.println("Log: " + mensagem);
    }
}
```

---

## 11. Mercado e Frameworks

Java permanece central em grandes sistemas corporativos, fintechs e aplicações Android (embora o Kotlin tenha se popularizado).

Frameworks como:

- **Spring**: para injeção de dependências, segurança e microserviços.
- **Hibernate**: para mapeamento objeto-relacional (ORM).
- **Apache Kafka**: para processamento de streams em larga escala.
- **JUnit**: para testes unitários.

Java evolui constantemente, com novas versões, melhor suporte para programação funcional (streams, lambdas) e um ecossistema vibrante.

---

**Autor**: Victor Reboredo  
**Contato**: victoreboredo.dev@gmail.com

---

🎯 **Objetivo**: Tornar o aprendizado mais fluido e prático. Bons estudos!
