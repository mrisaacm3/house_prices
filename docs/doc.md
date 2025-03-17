# 📊 Análise de Dados Imobiliários

## 🏠 1. Visão Geral

Este projeto realiza a análise de dados do mercado imobiliário utilizando:

- **Limpeza de dados** com Panda
- **Engenharia de atributos (Feature Engineering)**
- **Regressão Linear e Logística**
- **Análise Exploratória de Dados (EDA)**
- **Armazenamento em MySQL**
- **Visualização de dados no Power BI**
- **Mapas geoespaciais** com GeoPandas e Folium

Os dados são obtidos de conjuntos públicos armazenados localmente e processados para insights do setor imobiliário.

---

## 🎯 2. Objetivos e Insights de Mercado

### 2.1 Objetivos:

✅ **Analisar tendências de preços** para prever valorizações ou desvalorizações.

✅ **Identificar fatores que impactam preços**, como localização e infraestrutura.

✅ **Detectar regiões com alta valorização** e suas características.

✅ **Criar modelos preditivos** para estimar preços futuros.

✅ **Avaliar risco de investimento** com classificação logística.

✅ **Desenvolver dashboards interativos** para suporte a decisões.

### 2.2 Perguntas de Negócio:

❓ Quais regiões tiveram maior crescimento no preço de imóveis?

❓ Quais fatores mais influenciam nos preços?

❓ Quais bairros estão se valorizando mais?

❓ Como a distribuição de preços varia geograficamente?

❓ Como prever preços de imóveis em diferentes cenários?

❓ Existem padrões específicos de valorização por cidade ou estado?

---

## 📂 3. Estrutura do Repositório

```
🗂 house_prices/
	🗂 data/                # Dados brutos e processados
	    📝 train.csv
	    📝 test.csv
	    📝 sample_submission.csv
	
	🗂 scripts/             # Scripts SQL e Python
	    📝 criar_bd_mysql.sql
	    📝 processamento_dados.py
	    📝 extracao_dados.py
	
	🗂 dashboards/          # Dashboards no Power BI
	    📝 dashboard.pbix
	
	🗂 docs/                # Documentação
	  📝 guia_de_estilo.md
	
	📝 01_coleta_dados.ipynb
	📝 02_eda_visualizacao.ipynb
	📝 03_modelagem_preditiva.ipynb
	
	📝 README.md            # Documentação principal do projeto
```

---

## 📀 4. Coleta e Armazenamento de Dados

### 4.1 Estratégia de Coleta e Atualização de Dados

Antes de realizar a coleta de novos dados, o sistema verifica a existência do banco de dados e das tabelas. Se os dados já estiverem armazenados, a consulta é evitada. O fluxo segue:

1. **Verificar a existência do banco e tabelas**:
    - Se o banco não existir, ele é criado junto com as tabelas.
2. **Comparar os dados mais recentes**:
    - Se os dados mais novos já estiverem armazenados, a coleta é interrompida.
    - Caso contrário, os novos dados são adicionados.

### 4.2 Estrutura do Banco de Dados MySQL

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

---

## 🌐 5. Processamento e Análise de Dados

### 5.1 Limpeza de Dados e Feature Engineering

- **Remoção de valores nulos e duplicados** (`dropna`, `drop_duplicates`).
- **Conversão de tipos de dados** (`astype`).
- **Engenharia de atributos** (ex: "preço por metro quadrado").
- **Normalização e padronização** (`MinMaxScaler`, `StandardScaler`).
- **Filtragem de outliers** (`z-score`).

---

## 🔄 6. Visualização de Dados

### 6.1 Dashboard no Power BI

- **Indicadores principais:**
    - Tendência de preços por estado e município.
    - Impacto de fatores no valor do imóvel.
    - Distribuição de preços por região.
- **Fonte de dados:** MySQL conectado ao Power BI.

### 6.2 Mapas Geoespaciais com GeoPandas e Folium

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

---

## 📝 7. Considerações Finais

Este projeto integra conceitos de **ciência de dados**, **machine learning**, **business intelligence** e **visualização**, fornecendo insights acionáveis para o mercado imobiliário. A estrutura organizacional do repositório garante reprodutibilidade e escalabilidade para futuras análises e expansões.

---

🌟 **Desenvolvido para análise avançada de preços imobiliários!**