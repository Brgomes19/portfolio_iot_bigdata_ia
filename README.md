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
import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import requests

# Configuração da API
API_KEY = "ce6cf546f72b1a158ef7ef30f9f434da"  # Substitua pela sua chave da OpenWeatherMap
BASE_URL = "http://api.openweathermap.org/data/2.5/weather"

def get_weather(city):
    params = {"q": city, "appid": API_KEY, "units": "metric", "lang": "pt"}
    response = requests.get(BASE_URL, params=params)
    if response.status_code == 200:
        data = response.json()
        return {
            "cidade": data["name"],
            "temperatura": data["main"]["temp"],
            "descricao": data["weather"][0]["description"].capitalize(),
            "umidade": data["main"]["humidity"],
            "vento": data["wind"]["speed"],
        }
    return None

# Inicializando o aplicativo Dash
app = dash.Dash(__name__)
app.title = "Previsão do Tempo"

app.layout = html.Div([
    html.H1("Previsão do Tempo"),
    dcc.Input(id="cidade", type="text", value="São Paulo", placeholder="Digite a cidade"),
    html.Button("Buscar", id="botao"),
    html.Div(id="saida")
])

@app.callback(
    Output("saida", "children"),
    Input("botao", "n_clicks"),
    Input("cidade", "value")
)
def atualizar_temperatura(n, cidade):
    if cidade:
        dados = get_weather(cidade)
        if dados:
            return html.Div([
                html.H2(f"{dados['cidade']}") ,
                html.P(f"Temperatura: {dados['temperatura']}°C"),
                html.P(f"Descrição: {dados['descricao']}"),
                html.P(f"Umidade: {dados['umidade']}%"),
                html.P(f"Vento: {dados['vento']} m/s")
            ])
        else:
            return "Cidade não encontrada!"
    return "Digite uma cidade e clique em buscar."

if __name__ == "__main__":
    app.run(debug=True)

```
Agora, inicie o dashboard executando:
```sh
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
