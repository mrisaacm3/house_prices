# Guia de Estilo do Projeto

## 1. Introdução
Este guia define padrões e boas práticas para o desenvolvimento, documentação e versionamento do projeto **Análise do Mercado Imobiliário Brasileiro**. Seguir essas diretrizes garante código limpo, consistente e reprodutível.

---

## 2. Estrutura do Código
- **Cada módulo deve ter um propósito bem definido** (exemplo: coleta, processamento, modelagem).
- **Organizar código em funções e classes reutilizáveis**.
- **Evitar repetições de código** aplicando princípios de modularização e reutilização.

### Estrutura recomendada para scripts Python:
```python
import pandas as pd
import numpy as np

def carregar_dados(caminho):
    """Carrega os dados a partir de um arquivo CSV."""
    return pd.read_csv(caminho)

def processar_dados(df):
    """Executa limpeza e transformação dos dados."""
    df.dropna(inplace=True)
    return df

if __name__ == "__main__":
    dados = carregar_dados("data/dados_brutos.csv")
    dados_processados = processar_dados(dados)
    dados_processados.to_csv("data/dados_processados.csv", index=False)
```

---

## 3. Padrões de Nomeação
### 3.1 Variáveis e Funções
- **Usar nomes descritivos e claros**.
- **Seguir a convenção `snake_case`**.
- **Evitar abreviações desnecessárias**.

✔️ Exemplo correto:
```python
preco_medio_imovel = 350000.0
```
❌ Exemplo incorreto:
```python
p_m_i = 350000.0
```

### 3.2 Nomes de Arquivos
- **Os arquivos devem ser nomeados em `snake_case`**.
- **Scripts Python devem conter um verbo indicando ação** (exemplo: `processar_dados.py`).

### 3.3 Nomes de Colunas em DataFrames
- **Padronizar colunas sempre em minúsculas e sem espaços**.
- **Utilizar underscore (`_`) para separar palavras**.

✔️ Exemplo correto:
```python
df.columns = ["municipio", "estado", "area", "preco_medio"]
```
❌ Exemplo incorreto:
```python
df.columns = ["Município", "Estado", "Área", "Preço Médio"]
```

---

## 4. Boas Práticas de Versionamento
- **Commits devem ser pequenos e descritivos**.
- **Usar mensagens de commit padronizadas**:

✔️ Exemplo correto:
```
feat: adicionar função de normalização de dados
fix: corrigir erro de tipo na extração de dados
```

- **Criar branches específicas para cada funcionalidade**.
  - `feature/nova_funcionalidade`
  - `fix/correcao_de_bug`
  - `docs/atualizacao_documentacao`

---

## 5. Documentação do Código
- **Todos os scripts Python devem conter docstrings** no formato Google Style.
- **Cada função deve descrever claramente seu objetivo, parâmetros e retorno**.

✔️ Exemplo correto:
```python
def calcular_media_precos(df):
    """
    Calcula a média dos preços dos imóveis por município.

    Args:
        df (DataFrame): DataFrame contendo colunas 'municipio' e 'preco'.

    Returns:
        DataFrame: DataFrame com preços médios por município.
    """
    return df.groupby("municipio")["preco"].mean().reset_index()
```

---

## 6. Formatação de Código
- **Usar indentação de 4 espaços (padrão Python)**.
- **Manter um espaço antes e depois dos operadores (`=`, `+`, `-`, etc.)**.
- **Limite de 79 caracteres por linha** (exceto para URLs e strings longas).

✔️ Exemplo correto:
```python
preco_medio = (df["preco"].sum() / df["preco"].count())
```
❌ Exemplo incorreto:
```python
preco_medio=(df["preco"].sum()/df["preco"].count())
```

---

## 7. Estrutura dos Notebooks Jupyter
Os notebooks devem seguir esta estrutura padrão:
1. **Introdução**: Descrição do objetivo do notebook.
2. **Coleta de Dados**: Código de extração e fonte dos dados.
3. **Pré-processamento**: Limpeza e transformação dos dados.
4. **Exploração de Dados**: Visualizações e estatísticas.
5. **Modelagem Preditiva**: Aplicação de algoritmos de aprendizado de máquina.
6. **Conclusão**: Resumo dos resultados obtidos.

---

## 8. Considerações Finais
Seguir este guia de estilo garante código limpo, organizado e fácil de manter. Qualquer nova contribuição deve respeitar essas diretrizes para manter a padronização do projeto.