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
│-- 📄 .env                # Variáveis de ambiente
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
Crie um arquivo chamado `.env` e adicione o seguinte conteúdo:
```env
POSTGRES_USER=Brgomes19
POSTGRES_PASSWORD=Lili01040@
POSTGRES_DB=database
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
```

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
Crie o arquivo `app/dashboard.py` com o seguinte conteúdo:
```python
import streamlit as st
import pandas as pd
import plotly.express as px
from sqlalchemy import create_engine
from dotenv import load_dotenv
import os

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

DB_USER = os.getenv("POSTGRES_USER")
DB_PASSWORD = os.getenv("POSTGRES_PASSWORD")
DB_NAME = os.getenv("POSTGRES_DB")
DB_HOST = os.getenv("POSTGRES_HOST", "localhost")
DB_PORT = os.getenv("POSTGRES_PORT", "5432")

# Criar conexão com o banco de dados
engine = create_engine(f"postgresql://{DB_USER}:{DB_PASSWORD}@{DB_HOST}:{DB_PORT}/{DB_NAME}")

# Consulta SQL para buscar os dados
query = "SELECT dispositivo, temperatura, timestamp FROM leituras_iot"
df = pd.read_sql(query, engine)

# Convertendo timestamp para datetime
df["timestamp"] = pd.to_datetime(df["timestamp"])

# 📊 **Dashboard**
st.title("📊 Dashboard IoT - Análise de Temperatura")

# 📌 **Média de temperatura por dispositivo**
st.subheader("Média de Temperatura por Dispositivo")
media_temp = df.groupby("dispositivo")["temperatura"].mean().reset_index()
fig_media = px.bar(media_temp, x="dispositivo", y="temperatura", text_auto=".2f")
st.plotly_chart(fig_media)

# 📈 **Tendência das leituras ao longo do tempo**
st.subheader("Tendência das Leituras")
fig_tendencia = px.line(df, x="timestamp", y="temperatura", color="dispositivo", markers=True)
st.plotly_chart(fig_tendencia)

# 🔍 **Gráficos interativos**
st.subheader("Análise Exploratória")
dispositivo_selecionado = st.selectbox("Escolha um dispositivo:", df["dispositivo"].unique())
df_filtrado = df[df["dispositivo"] == dispositivo_selecionado]
fig_interativo = px.scatter(df_filtrado, x="timestamp", y="temperatura", title=f"Temperatura do {dispositivo_selecionado}", color="temperatura")
st.plotly_chart(fig_interativo)

# Finalização
st.write("📌 **Dashboard atualizado em tempo real!**")
```
Agora, inicie o dashboard executando:
```sh
streamlit run app/dashboard.py
```
Acesse o dashboard no navegador: [http://localhost:8501](http://localhost:8501)

## 📊 Visualização dos Dados
O dashboard exibe:
✔ Média de temperatura por dispositivo IoT
✔ Tendência das leituras ao longo do tempo
✔ Gráficos interativos para análise dos dados

## 📜 Licença
Este projeto é open-source sob a licença MIT.
