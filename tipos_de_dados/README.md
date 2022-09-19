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

Um *Gauge* é um tipo de métrica que representa um valor numérico único que tem a capacidade de arbitrariamente ser acrescido ou decrescido. Este tipo de métrica é utilizada para representação de valores que podem ser medidos, tais como temperaturas ou uso de algum recurso computacional (memória, cpu, disco). Ela também pode ser utilizada para valores que podem subir ou descer e representam uma contagem momentânea, tal como a quantidade de acessos concorrentes.

- Python:
  ```python
  from prometheus_client import Gauge

  g = Gauge('my_inprogress_requests', 'Description of gauge')
  g.inc()      # Increment by 1
  g.dec(10)    # Decrement by given value
  g.set(4.2)   # Set to a given value
  ```

- Ruby:
  ```ruby
  require 'prometheus/client'

  gauge = Prometheus::Client::Gauge.new(:room_temperature_celsius, docstring: '...', labels: [:room])

  # set a value
  gauge.set(21.534, labels: { room: 'kitchen' })
  # retrieve the current value for a given label set
  gauge.get(labels: { room: 'kitchen' })
  # => 21.534
  # increment the value (default is 1)
  gauge.increment(labels: { room: 'kitchen' })
  # => 22.534
  # decrement the value by a given value
  gauge.decrement(by: 5, labels: { room: 'kitchen' })
  # => 17.534
  ```

## Histogram

Um *histogram* representa uma amostra de observações, tais como a duração de requisições ou tamanhos de repostas, e faz a contagem deles em estruturas configuráveis denominadas de buckets. Ele também tem a capacidade de prover a somre de todos os valores observados.

O *histogram* representa um tipo de métrica um pouco mais elaborada do que os *counters* e "gauges". A partir de uma métrica do tipo *histogram* com o nome *<basename>* temos a exposição de múltiplas séries de tempo durante um mesmo *scrape*:

* Contadores acumulativos para os buckets que contêm as observações e expostos com o seguinte formato *<basename>_bucket{le="<upper inclusive bound>"}*;
* A soma de todos os vlaores observados, expostos como *<basename>_sum*;
* A contagem de eventos que foram observados, expostos como *<basename>_count*;

Neste tipo de métrica é possível fazer uso da função *histogram_quantile()* para calcular quantiles de *histograms* ou até mesmo agregações de *histograms*. Os histograms também podem ser utilizados para efetuar o score **Apdex** (Apdex - Application Performance Index - é um padrão aberto desenvolvido por um conjunto de empresas para mensurar a performance de aplicações).

- Python:
  ```python
  from prometheus_client import Histogram
  
  h = Histogram('request_latency_seconds', 'Description of histogram')
  h.observe(4.7)    # Observe 4.7 (seconds in this case)
  ```

- Ruby:
  ```ruby
  require 'prometheus/client'

  histogram = Prometheus::Client::Histogram.new(:service_latency_seconds, docstring: '...', labels: [:service])

  # record a value
  histogram.observe(Benchmark.realtime { service.call(arg) }, labels: { service: 'users' })
  # retrieve the current bucket values
  histogram.get(labels: { service: 'users' })
  # => { 0.005 => 3, 0.01 => 15, 0.025 => 18, ..., 2.5 => 42, 5 => 42, 10 = >42 }
  ```

## Summary

- Python:
  ```python
  from prometheus_client import Summary
  
  s = Summary('request_latency_seconds', 'Description of summary')
  s.observe(4.7)    # Observe 4.7 (seconds in this case)
  ```
- Ruby:
  ```ruby
  require 'prometheus/client'

  summary = Prometheus::Client::Summary.new(:service_latency_seconds, docstring: '...', labels: [:service])

  # record a value
  summary.observe(Benchmark.realtime { service.call() }, labels: { service: 'database' })
  # retrieve the current sum and total values
  summary_value = summary.get(labels: { service: 'database' })
  summary_value['sum'] # => 123.45
  summary_value['count'] # => 100
  ```
  
## Referências
1. https://prometheus.io/docs/concepts/metric_types/ 
2. https://pt.wikipedia.org/wiki/Fun%C3%A7%C3%A3o_mon%C3%B3tona 
