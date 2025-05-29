# Estudo Dirigido: Java e Go para Sistemas Críticos

## 1. Padrões de Projeto para Sistemas Críticos

### Java: Padrão CQRS com Spring Boot + Axon Framework

**Visão geral**: CQRS separa leitura e escrita, usando Event Sourcing para reconstruir estado.

**Implementação**:

- Criação de Aggregates com `@Aggregate`.
- Event Store em bancos como Postgres ou MongoDB.
- Projeções com `@ProcessingGroup` e `@EventHandler`.

**Exemplo**:

```java
@Aggregate
public class PaymentAggregate {
  @AggregateIdentifier
  private String paymentId;

  @CommandHandler
  public PaymentAggregate(CreatePaymentCommand cmd) {
    AggregateLifecycle.apply(new PaymentCreatedEvent(cmd.getPaymentId()));
  }

  @EventSourcingHandler
  public void on(PaymentCreatedEvent event) {
    this.paymentId = event.getPaymentId();
  }
}
```

**Benefícios**: alta leitura, forte consistência em escrita.

---

### Go: Padrão Saga com Compensação Transacional

**Visão geral**: Saga gerencia transações distribuídas com passos compensáveis.

**Implementação**:

- Orquestração com NATS.
- Workers consomem mensagens e executam etapas.

**Exemplo**:

```go
func paymentWorker(nc *nats.Conn) {
  nc.Subscribe("payment.created", func(m *nats.Msg) {
    var payment Payment
    json.Unmarshal(m.Data, &payment)
    // processamento e compensação
    nc.Publish("payment.compensation", m.Data)
  })
}
```

**Consistência**: eventual em todas as etapas.

---

### Comparação de Eficiência

| Aspecto          | Java (CQRS)                       | Go (Saga)                           |
|------------------|------------------------------------|------------------------------------|
| Throughput       | Alto para leitura                 | Alto para workflows distribuídos   |
| Consistência     | Forte (comando), eventual (leitura)| Eventual em todos os passos        |
| Escalabilidade   | Queries escalam facilmente         | Workers escalam horizontalmente    |
| Complexidade     | Alta (eventos, projeções)         | Alta (compensações, idempotência)  |

---

## 2. Integração com Sistemas Legacy e Cloud

### Java com IBM MQ

- Uso de `spring-jms` ou `ibm-mq-spring-boot-starter`.
- Exemplo com `@JmsListener` para consumir mensagens.

**Serialização**: protobuf (eficiente), JSON (mais interoperável).  
**Resiliência**: Resilience4j com circuit breakers e retries.

---

### Go com AWS Lambda e SQS

- Uso de `aws-sdk-go`.
- Resiliência com `cenkalti/backoff` para retries exponenciais.

**Monitoramento**:  
- Java: Micrometer + Prometheus.  
- Go: promhttp.

---

## 3. Performance Tuning e Escalabilidade

### Java

- Ajustes de heap (`-Xms`, `-Xmx`).
- GCs modernos: ZGC, Shenandoah.
- Profiling com Async Profiler.

**Exemplo real**: Redução de latência de 500ms para <50ms trocando GC e ajustando heap.

---

### Go

- Uso de `sync.Pool` para reuso de objetos.
- Profiling com pprof.

**Exemplo real**: API com latência alta por alocações de slices. Otimização com `sync.Pool` reduziu para <50ms.

---

## Conclusão

✅ Java: Axon, JVM tuning, Micrometer.  
✅ Go: Saga, pprof, mensageria (NATS, SQS).  
✅ Integração: Prometheus + Grafana.

---

**Autor**: Victor Reboredo  
**Contato**: victoreboredo.dev@gmail.com

---

🎯 **Objetivo**: Tornar o aprendizado mais fluido e prático. Bons estudos!