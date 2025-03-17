## 1. Visão Geral

Este projeto analisa dados imobiliários brasileiros, utilizando técnicas de **limpeza de dados, engenharia de atributos (feature engineering), regressão linear e logística, análise exploratória de dados (EDA)** e visualização geoespacial com **GeoPandas e Folium**. O repositório inclui coleta de dados da **API do IBGE**, armazenamento em **MySQL**, processamento com **Python e Pandas**, e visualização em **Power BI**.

## 2. Objetivos e Insights de Mercado

### 2.1 Objetivos

- **Analisar a tendência de preços no mercado imobiliário brasileiro**: Identificar padrões e prever possíveis valorizações ou desvalorizações.
- **Determinar os fatores que impactam os preços dos imóveis**: Avaliar variáveis como localização, metragem, número de quartos e infraestrutura urbana.
- **Identificar hotspots de valorização**: Mapear regiões em ascensão e as características que impulsionam essa valorização.
- **Criar um modelo preditivo de preço**: Usar regressão linear para prever valores de imóveis em diferentes cenários.
- **Avaliar o risco de investimento em diferentes regiões**: Aplicar classificação logística para estimar a probabilidade de valorização dos imóveis.
- **Desenvolver dashboards interativos para tomada de decisão**: Criar visualizações intuitivas que ajudem investidores, corretores e compradores a tomar decisões informadas.

### 2.2 Perguntas de Negócio a Explorar

1. **Quais regiões apresentam maior crescimento nos preços de imóveis nos últimos anos?**
2. **Quais fatores têm maior impacto nos preços dos imóveis?** (Exemplo: proximidade a metrô, infraestrutura urbana, segurança)
3. **Quais bairros estão se tornando mais valorizados e por quê?**
4. **Como a distribuição de preços varia geograficamente?**
5. **Qual a previsão de preços para diferentes faixas de imóveis?**
6. **Há padrões de valorização específicos para determinadas cidades ou estados?**

## 3. Estrutura do Repositório

```
📁 mercado_imobiliario_brasil/
│
├── 📁 data/                # Dados brutos e processados
│   ├── 📄 dados_brutos.csv
│   ├── 📄 dados_processados.csv
│   ├── 📄 coordenadas_municipios.csv
│
├── 📁 notebooks/           # Jupyter Notebooks para EDA e Modelagem
│   ├── 📄 01_coleta_dados.ipynb
│   ├── 📄 02_eda_visualizacao.ipynb
│   ├── 📄 03_modelagem_preditiva.ipynb
│
├── 📁 scripts/             # Scripts SQL e Python
│   ├── 📄 criar_bd_mysql.sql
│   ├── 📄 processamento_dados.py
│   ├── 📄 extracao_dados_ibge.py
│
├── 📁 dashboards/          # Arquivos Power BI
│   ├── 📄 dashboard_powerbi.pbix
│
├── 📁 docs/                # Documentação
│   ├── 📄 guia_de_estilo.md
│
└── 📄 README.md            # Documentação principal do projeto

```

## 4. Coleta e Armazenamento de Dados

### 4.1 Estratégia de Coleta e Atualização de Dados

Antes de realizar a coleta de novos dados na API do IBGE, o sistema verifica a existência do banco de dados e das tabelas necessárias. Caso os dados já estejam armazenados, a consulta é evitada. O processo segue os seguintes passos:

1. **Verificar a existência do banco de dados e das tabelas**:
    - Se o banco ainda não existir, ele será criado junto com as tabelas necessárias.
    - Se já existir, passa-se à próxima etapa.
2. **Comparar o último dado armazenado com a última entrada disponível na API**:
    - A comparação pode ser feita utilizando a data mais recente presente no banco.
    - Caso os dados mais recentes já estejam armazenados, a coleta é interrompida.
    - Caso haja novos dados, a API será consultada e apenas os dados novos serão adicionados.

### 4.2 Fonte: API do IBGE

Os dados são extraídos da API do IBGE, especificamente do endpoint de dados agregados.

- **URL:** `https://servicodados.ibge.gov.br/api/v3/agregados/`
- **Dados Extraídos:** Preços médios de imóveis, dados socioeconômicos por município.
- **Script de Extração:** `scripts/extracao_dados_ibge.py`

### 4.3 Estrutura do Banco de Dados MySQL

O banco **mercado_imobiliario** armazena informações sobre imóveis e localização:

```sql
CREATE DATABASE IF NOT EXISTS mercado_imobiliario;
USE mercado_imobiliario;

CREATE TABLE IF NOT EXISTS imoveis (
    id INT AUTO_INCREMENT PRIMARY KEY,
    municipio VARCHAR(100),
    estado CHAR(2),
    area FLOAT,
    quartos INT,
    banheiros INT,
    preco FLOAT,
    latitude FLOAT,
    longitude FLOAT,
    data_registro DATE
);
```

## 5. Processamento e Análise de Dados

### 5.1 Limpeza de Dados e Feature Engineering

- **Remoção de valores nulos e duplicados** (`dropna`, `drop_duplicates`).
- **Conversão de tipos de dados** (`astype`).
- **Engenharia de atributos** (criação de variáveis derivadas como "preço por metro quadrado").
- **Normalização e padronização** (`MinMaxScaler`, `StandardScaler`).
- **Filtragem por limites estatísticos** para remoção de outliers (`z-score`).

## 6. Visualização de Dados

### 6.1 Dashboard no Power BI

- **Indicadores chave:**
    - Tendência de preços por estado e município.
    - Análise de fatores que impactam o valor do imóvel.
    - Gráficos interativos de distribuição de preços.
- **Origem dos dados:** MySQL conectado via Power BI.

### 6.2 Mapas Geoespaciais com GeoPandas e Folium

- **Carregamento de coordenadas de municípios:** `GeoPandas.read_file()`.
- **Criação de mapas interativos:** `Folium.Map()` com marcadores de preço.

```python
import geopandas as gpd
import folium

# Carregar dados geoespaciais
gdf = gpd.read_file("data/municipios.geojson")

# Criar mapa interativo
mapa = folium.Map(location=[-15.7801, -47.9292], zoom_start=5)
for _, row in gdf.iterrows():
    folium.Marker(
        location=[row["latitude"], row["longitude"]],
        popup=f"{row['municipio']}: R${row['preco_medio']:.2f}"
    ).add_to(mapa)

mapa.save("mapa_interativo.html")
```

## 7. Considerações Finais

Este projeto unifica conceitos fundamentais de **ciência de dados**, **machine learning**, **business intelligence** e **visualização** em um estudo aplicável ao mercado imobiliário brasileiro. A documentação e estrutura são organizadas para garantir reprodutibilidade e escalabilidade, permitindo uma análise aprofundada de investimentos imobiliários e tendências de mercado.