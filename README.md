# Documentação do Projeto de Monitoramento de Sensores

Este projeto é composto por dois módulos principais: uma aplicação backend em Flask (`app.py`) que gerencia dados de sensores e uma ferramenta em Python (`simulador_sensores.py`) que simula dados de sensores e os envia para o backend. Abaixo estão as instruções detalhadas de instalação, configuração e uso, bem como uma visão geral da arquitetura.

---

## Tabela de Conteúdos

1. [Requisitos](#requisitos)
2. [Instalação e Configuração](#instalação-e-configuração)
3. [Arquivos do Projeto](#arquivos-do-projeto)
4. [Endpoints da API (app.py)](#endpoints-da-api-apppy)
5. [Simulador de Sensores (simulador_sensores.py)](#simulador-de-sensores-simulador_sensorespy)
6. [Fluxo de Dados e Estrutura](#fluxo-de-dados-e-estrutura)

---

## 1. Requisitos

- Python 3.7 ou superior
- Flask (para o backend)
- SQLite (banco de dados embutido no Python)
- Requests (biblioteca para simulação de requisições HTTP)
  
## 2. Instalação e Configuração

1. **Clone o repositório:**

   ```bash
   git clone https://github.com/gafdot/gafdot-dados-sensores-flask-python
   cd gafdot-dados-sensores-flask-python
   ```

2. **Instale as dependências do projeto:**

   ```bash
   pip install flask requests
   ```

3. **Inicie o servidor Flask:**

   ```bash
   python app.py
   ```

O servidor será iniciado em `http://127.0.0.1:5000`.

---

## 3. Arquivos do Projeto

- **app.py**: Implementa a aplicação backend com Flask, fornecendo uma API RESTful para manipulação de dados de sensores.
- **simulador_sensores.py**: Simula dados de sensores e os envia para o backend Flask.
- **templates/index.html** e **templates/graficos.html**: Páginas para visualização dos dados armazenados e para exibição de gráficos de dados dos sensores.

---

## 4. Endpoints da API (app.py)

### Inicialização do Banco de Dados

O banco de dados SQLite é inicializado na execução do `app.py`. A função `init_db()` cria a tabela `dados_sensores` se ela ainda não existir, garantindo que o sistema esteja preparado para armazenar dados de sensores.

### Endpoints Disponíveis

#### `POST /dados-sensores`

Insere novos dados de sensores no banco de dados.

- **Parâmetros JSON**: `{ "sensor_id": <int>, "temperatura": <float>, "umidade": <float> }`
- **Resposta**: `{ "message": "Dados inseridos com sucesso" }` com código de status 201.

#### `GET /dados-sensores`

Retorna todos os dados de sensores armazenados.

- **Resposta**: Lista de dados no formato `[{ "id": <int>, "sensor_id": <int>, "temperatura": <float>, "umidade": <float>, "timestamp": <string> }, ...]`.

#### `DELETE /limpar-dados`

Limpa todos os dados armazenados na tabela `dados_sensores`.

- **Resposta**: `{ "message": "Dados limpos com sucesso" }` com código de status 200.

#### `GET /dados-sensores-json`

Fornece dados formatados para visualização gráfica.

- **Resposta**: JSON com listas de `timestamp`, `temperatura` e `umidade`.

#### `GET /`

Renderiza a página principal (`index.html`) com uma tabela dos dados de sensores armazenados.

#### `GET /graficos`

Renderiza a página de gráficos (`graficos.html`) para visualização de dados.

---

## 5. Simulador de Sensores (simulador_sensores.py)

O `simulador_sensores.py` é um script que simula dois sensores, gerando dados aleatórios de temperatura e umidade a cada 10 segundos e enviando-os para o backend.

### Estrutura e Funcionamento

1. **URL do Backend**: O script se conecta ao endpoint `POST /dados-sensores` em `http://127.0.0.1:5000`.
2. **Geração de Dados**: A função `simular_sensores()` gera aleatoriamente:
   - `sensor_id`: ID do sensor, alternando entre 1 e 2.
   - `temperatura`: Valor entre 20.0 e 30.0 °C.
   - `umidade`: Valor entre 40% e 70%.
3. **Envio de Dados**: Após gerar os valores, o script faz uma requisição `POST` ao backend.
4. **Intervalo de Envio**: O script aguarda 10 segundos antes de gerar e enviar o próximo conjunto de dados.

### Como Executar

Em um terminal separado, execute:

```bash
python simulador_sensores.py
```

---

## 6. Fluxo de Dados e Estrutura

### Estrutura do Banco de Dados

O banco de dados SQLite possui uma única tabela `dados_sensores` com as seguintes colunas:

- **id**: Identificador único para cada registro.
- **sensor_id**: Identificador do sensor (ex.: 1 ou 2).
- **temperatura**: Valor de temperatura coletado pelo sensor.
- **umidade**: Valor de umidade coletado pelo sensor.
- **timestamp**: Registro de data e hora da coleta.

### Fluxo de Dados

1. **Simulação de Dados**: O script `simulador_sensores.py` gera dados de temperatura e umidade.
2. **Envio para o Backend**: Os dados simulados são enviados ao endpoint `/dados-sensores` usando uma requisição `POST`.
3. **Armazenamento e Visualização**:
   - O backend armazena os dados no banco de dados SQLite.
   - O endpoint `/dados-sensores` exibe todos os dados de sensores em formato JSON.
   - O endpoint `/dados-sensores-json` retorna dados em formato adequado para visualização gráfica.
   - A página `/graficos` exibe gráficos gerados a partir dos dados armazenados.
