# Tipos de dados (métricas)

As bibliotecas de cliente do Prometheus podem oferecer quatro tipos básicos de métricas. Estas métricas são importantes para o entendimento e construção dos exporters, tendo em vista que os tipos serão utilizados no momento da instanciação das métricas através das bibliotecas. O servidor do Prometheus ainda não faz uso destes tipos de informações, fazendo com que o dado seja transformado em uma série de tempo não tipada. Na documentação do Prometheus é feita uma consideração de que isto pode mudar no futuro. 

## Counter (Contador)

A métrica do tipo counter representa um valor monótono que somente pode ser incrementado ou resetado para zero quando reiniciado. A métrica vem da representação matemática de funções monótonas, que possuem como característica a preservação ou inversão da sua relação de ordem. A métrica counter, representa assim, um tipo de função monótona crescente.

Como exemplos podemos ter as seguintes situações de uso: representação do número de requisições a um servidor, quantidade de tarefas completas, número de determinados tipos de erros, etc.

Abaixo segue um exemplo de uso dessa métrica com clintes em Ruby e Python

- Python:
  ```python
  from prometheus_client import Counter

  c = Counter('my_failures', 'Description of counter')
  c.inc()     # Increment by 1
  c.inc(1.6)  # Increment by given value
  ```

- Ruby:
  ```ruby
  require 'prometheus/client'

  counter = Prometheus::Client::Counter.new(:service_requests_total, docstring: '...', labels: [:service])

  # increment the counter for a given label set
  counter.increment(labels: { service: 'foo' })
  # increment by a given value
  counter.increment(by: 5, labels: { service: 'bar' })
  # get current value for a given label set
  counter.get(labels: { service: 'bar' })
  # => 5
  ```
  
## Gauge

## Histogram

## Summary

## Referências
[1] https://prometheus.io/docs/concepts/metric_types/
[2] https://pt.wikipedia.org/wiki/Fun%C3%A7%C3%A3o_mon%C3%B3tona
