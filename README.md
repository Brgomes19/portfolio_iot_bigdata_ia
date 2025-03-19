# 📌 Portfólio: Disruptive Architectures - IoT, Big Data e IA

## 🚀 Sobre o Projeto
Este projeto demonstra a implementação de um **pipeline de dados** que coleta leituras de temperatura de dispositivos IoT, processa essas informações e as armazena em um banco de dados PostgreSQL utilizando Docker. Além disso, um **dashboard interativo em Streamlit** permite a visualização e análise desses dados.

## 📂 Estrutura do Repositório
```plaintext
📁 portfolio_iot_bigdata_ia
│-- 📂 app/                # Código do dashboard
│-- 📂 data/               # Exemplo de dados IoT
│-- 📂 scripts/            # Pipeline de processamento
│-- 📄 Dockerfile          # Configuração do container
│-- 📄 docker-compose.yml  # Orquestração dos serviços
│-- 📄 .env.example        # Exemplo de variáveis de ambiente
│-- 📄 README.md           # Documentação do projeto
```

## 🛠️ Tecnologias Utilizadas
- **Python** (pandas, psycopg2, sqlalchemy, streamlit, plotly, python-dotenv)
- **Docker & Docker Compose**
- **PostgreSQL**
- **Git & GitHub**

## 🔧 Configuração e Execução
### 1️⃣ Clone o repositório
```sh
git clone https://github.com/SEU_USUARIO/portfolio_iot_bigdata_ia.git
cd portfolio_iot_bigdata_ia
```

### 2️⃣ Configure o arquivo `.env`
Crie um arquivo chamado `.env` baseado no `.env.example` e preencha os valores adequados.

### 3️⃣ Suba os serviços com Docker
Crie o arquivo `docker-compose.yml` com o seguinte conteúdo:
```yaml
version: '3.8'

services:
  postgres:
    image: postgres:latest
    container_name: postgres_iot
    restart: always
    env_file:
      - .env
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```
Agora execute o comando:
```sh
docker-compose up -d
```
Isso iniciará o PostgreSQL e o ambiente necessário para o processamento dos dados.

### 4️⃣ Execute o pipeline de dados
```sh
python scripts/process_data.py
```
Isso carregará os dados IoT no banco de dados PostgreSQL.

### 5️⃣ Inicie o dashboard
 http://127.0.0.1:8050/
streamlit run app/dashboard.py
```
Acesse o dashboard no navegador: [[http://localhost:8501](http://localhost:8501)](http://127.0.0.1:8050/)

## 📊 Visualização dos Dados
O dashboard exibe:
✔ Média de temperatura por dispositivo IoT
✔ Tendência das leituras ao longo do tempo
✔ Gráficos interativos para análise dos dados

## 📜 Licença
Este projeto é open-source sob a licença MIT.
