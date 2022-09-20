# Descomplicando Prometheus
Respositório criado para o desafio da segunda semana do curso Descomplicando Prometheus da LinuxTips.

# Organização

Este repositório está organizado em três partes:

1. [Tutorial de instalação do Prometheus - Linux](./tutorial/README.md)
2. [Exemplos de arquivos de configuração do Prometheus](./conf/README.md)
3. [Apresentação dos tipos de dados disponíveis no Prometheus](./tipos_de_dados/README.md)

# O que é o Prometheus

Prometheus é uma solução de monitoramento de de serviços e sistemas. Ele é capaz de efetuar a coleta de métricas a partir de um conjunto de máquinas (targets) pré-definidos em um intervalo de tempo, além de avaliar expressões com regras, apresentar os resultados em tela (possibilidade de apresentação em forma gráfica) e disparar alertas se algumas condições forem observadas ao longo do tempo (através do AlertManager).

Ele possui algumas características básicas que o diferencia de outras soluções de monitoramento:

* Modelo multidimensional de dados (séries de tempos definidas pelos nomes das métricas e um conjunto de chave/valor);
* Uma poderosa e flexível linguagem de consulta para fazer uso do modelo multidimensional (PromQL);
* Não há dependência em storages distribuídos, fazendo com que instalações em um único servidor sejam autônomas;
* A coleta das séries de tempos nas máquinas alvo acontecem num modelo via PULL sobre o HTTP;
* As máquinas alvo podem utilizar o serviço de descoberta automática ou configuradas de forma estática;
* Suporte para configurações de forma federada hierárquica ou horizontal;

![Prometheus](images/Arquitetura-Prometheus.svg)

Referência: https://quay.io/repository/prometheus/prometheus
