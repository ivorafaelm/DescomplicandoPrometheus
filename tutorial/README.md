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
