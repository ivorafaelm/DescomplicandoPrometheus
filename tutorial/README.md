# Tutorial de instalação do Prometheus - Linux

O processo de instalação do Prometheus é bastante flexível e oferece uma gama de possibilidades. A instalação pode ser efetuada, conforme disponível na documentação oficial da solução, através de uma das seguintes formas:

## 1. Através do uso de binários pré-compilados:

Os binários pré-compilados para os componenentes oficiais do Prometheus são disponibilizados no próprio site da solução e podem ser acessados através da [seção de downloads](https://prometheus.io/download/).

Os principais componentes disponibilizados para downaload são:

* [Prometheus](https://github.com/prometheus/prometheus);
* [AlertManager](https://github.com/prometheus/alertmanager);
* [BlackBox Exporter](https://github.com/prometheus/blackbox_exporter);
* [Consul Exporter](https://github.com/prometheus/consul_exporter);
* [Graphite Exporter](https://github.com/prometheus/graphite_exporter);
* [HAProxy Exporter](https://github.com/prometheus/haproxy_exporter);
* [MemCached Exporter](https://github.com/prometheus/memcached_exporter);
* [MySQLD Exporter](https://github.com/prometheus/mysqld_exporter);
* [Node Exporter](https://github.com/prometheus/node_exporter);
* [Push Gateway](https://github.com/prometheus/pushgateway);
* [StatsD Exporter](https://github.com/prometheus/statsd_exporter);

## 2. A partir do código fonte (source):

## 3. Utilizando uma solução em container Docker:

Atualmente todos os serviços do Prometheus estão disponíveis em imagens Docker e podem ser baixados através do [Docker Hub](https://hub.docker.com/r/prom/prometheus/).

Executar o Prometheus em um container Docker é extremamente simples. O seguinte comando abaixo pode ser usado como referência:

```docker
  docker run -p 9090:9090 prom/prometheus
```

Este comando fará com que um container contendo a imagem do Prometheus seja executado, tendo como base uma configuração padrão e o serviço exposto na porta 9090. 

A imagem básica do Prometheus disponibilizada através do Docker Hub utiliza um volume para armazenar as métricas correntes. Para ambientes de produção é altamente recomendado a utilização de *volumes nomeados* (*named volumes*) para facilitar o gerenciamento dos dados em operações de atualização do Prometheus.

*Obs.* Volumes nomeados ou *named volumes* são volumes em que o Docker faz o gerenciamento do local de criação, além de, neste caso, fazer a associação com um nome pré-estabelecido. Os *named volumes* podem ser criados e referenciados de acordo com o exemplo abaixo:

```docker
  docker volume create volumenomeado

  docker run -v volumenomeado:/caminho/do/volume/no/container
```

**Volumes e bind-mount**

Bind-mount do arquivo prometheus.yml a partir do host:

```docker
  docker run \
    -p 9090:9090 \
    -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
    prom/prometheus
```

Bind-mount do diretório do host contendo o arquivo prometheus.yml para o container em /etc/prometheus:

```docker
  docker run \
    -p 9090:9090 \
    -v /path/to/config:/etc/prometheus \
    prom/prometheus
```

**Imagens customizadas**

Para evitar o gerenciamento de um arquivo de configuração no próprio host ou através da montagem de um volume, a configuração do Prometheus pode ser customizada e acrescentada em uma imagem Docker customizada. Esta abordagem funciona muito bem para configurações que podem ser utilizada em múltiplos ambientes.

Para a construção de uma imagem customizada é interessante a criação de um diretório contendo o arquivo de configuração do Prometheus (prometheus.yml). Neste mesmo diretório é necessário efetuar a criação de um Dockerfile com o seguinte conteúdo:

```docker
  FROM prom/prometheus
  ADD prometheus.yml /etc/prometheus/
```

Após a criação e customização do Dockerfile é necessário efetuar o build da imagem (no shell), conforme mostrado abaixo:

```docker
  docker build -t my-prometheus .
  docker run -p 9090:9090 my-prometheus
```

## 4. Através de um sistema de gerenciamento de configuração:

* *Ansible:* [Cloud Alchemy/ansible-prometheus](https://github.com/cloudalchemy/ansible-prometheus)
* *Chef:* [rayrod2030/chef-prometheus](https://github.com/elijah/chef-prometheus)
* *Puppet:* [puppet/prometheus](https://forge.puppet.com/puppet/prometheus)
* *SaltStack:* [saltstack-formulas/prometheus-formula](https://github.com/saltstack-formulas/prometheus-formula)

*Obs.* Algumas soluções também fazem uso das opções disponibilizadas pelos sistemas de gerenciamento de configuração, sendo capazes de entregar essa forma de instalação através de uma interface própria, como é o caso do [SUSE Manager](https://www.suse.com/pt-br/products/suse-manager/). Nele as fórmulas do Salt Stack ficam disponíveis para aplicação em uma máquina ou conjunto de máquinas, e podem compreender inclusive a instalação e configuração de exporters (tudo efetuado através da interface, facilitando o uso destas funções para as equipes que administram os servidores).

## Referências:
1. https://prometheus.io/docs/prometheus/latest/installation/
2. https://prometheus.io/download/
3. https://quay.io/repository/prometheus/prometheus
