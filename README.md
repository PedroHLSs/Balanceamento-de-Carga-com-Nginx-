# Balanceamento de Carga com Nginx e Docker

Este projeto demonstra alta disponibilidade com dois servidores web idênticos e um balanceador de carga Nginx atuando como proxy reverso.

## Arquitetura
- **nginx-lb**: recebe as requisições na porta 80 e distribui para os backends.
- **web1 / web2**: containers Nginx servindo páginas estáticas.
- **Rede**: `backend-net` (bridge) para comunicação isolada entre os containers.

## Pré-requisitos
- Docker
- Docker Compose

## Estrutura do projeto
- `docker-compose.yml`
- `nginx/nginx.conf`
- `web1/index.html`
- `web2/index.html`

## Como executar
1. Suba o ambiente:
   ```bash
   docker compose up -d
   ```
2. Acesse `http://localhost` e atualize a página para observar o balanceamento entre os servidores.

## Configuração do Nginx (Proxy Reverso)
O arquivo `nginx/nginx.conf` define um `upstream` com `web1` e `web2`. O algoritmo padrão do Nginx é **Round Robin**, distribuindo as requisições entre os servidores disponíveis.

## Teste de falha (tolerância a indisponibilidade)
1. Derrube propositalmente um servidor:
   ```bash
   docker compose stop web1
   ```
2. Acesse `http://localhost` e confirme que o serviço continua respondendo via `web2`.
3. Para restaurar:
   ```bash
   docker compose start web1
   ```