# Desafio O2B Novembro 2023 - Atividade Prática Observabilidade

Desafio para conclusão do curso de observabilidade ministrado pelo Prof. Patrick em Novembro de 2023.

### Objetivo do Laboratório:

Criar um ambiente de observabilidade usando Prometheus e Grafana para monitorar uma aplicação de exemplo.<br>
Link do desafio <https://github.com/patrickjcardoso/desafio_o11y>

> #### Technologies Used
>
> - Linux (Ubuntu 22.04LTS)
> - Docker-Compose
> - Phyton Application
> - Prometheus
> - Grafana
> - AWS EC2

### Passos:


1. ## Passo 1: Instalação do Prometheus e Grafana
    1. Conectar na ECS via SSH
        ```
       ssh -i "o2b_key.pem" ubuntu@ec2-54-159-107-52.compute-1.amazonaws.com
       ```
    2. Baixe o Docker Compose e instale-o em sua máquina se você ainda não o tiver:
       ```
         $ sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
         $ sudo chmod +x /usr/local/bin/docker-compose
       ```   
    3. Clone o repositório e acesse o diretório do laboratório:
       ```
         $ git clone https://github.com/patrickjcardoso/desafio_o11y.git
         $ cd desafio_o11y/observability-lab
       ```
   4. Crie um arquivo docker-compose.yml para definir os serviços Prometheus e Grafana:
    ```
      version: '3'
      services:
        prometheus:
          image: prom/prometheus
          volumes:
          - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
          command:
          - '--config.file=/etc/prometheus/prometheus.yml'
          ports:
            - '9090:9090'
          network_mode: "host"
      
        grafana:
          image: grafana/grafana
          ports:
          - '3000:3000'
          network_mode: "host"
    ```
    2. ## Passo 2: Configurando a aplicação de exemplo
       1. Instalar pip
          ```
          sudo apt install pip
          ``` 
       2. Instalar Flask e o Prometheus Client Python.
          ```
          pip install Flask prometheus_client
          ```
       3. Acesso o diretório da aplicação:
          ```
          cd python-app
          ```
       4. Liberar a porta 3001 na Grupo de Segurança
       5. Testando a aplicação python.
          Sua aplicação estará disponível em <http://ec2-54-159-107-52.compute-1.amazonaws.com:3001>. Você pode acessar a página inicial e também verificar as métricas expostas em <http://ec2-54-159-107-52.compute-1.amazonaws.com:3001/metrics>.
