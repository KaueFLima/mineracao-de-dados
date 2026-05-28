# Projeto de Mineração de Dados de Queimadas

Este projeto realiza mineração e análise de dados de queimadas no Brasil para os anos de 2023 a 2025. Ele extrai dados do BigQuery usando o pacote `basedosdados`, processa e salva um cache local em Parquet, gera mapas interativos com clusterização espacial e cria gráficos estatísticos.

## Estrutura do projeto

- `config.py` - Configurações centrais: nome do arquivo Parquet, query SQL e parâmetros de mineração.
- `pipeline.py` - Funções principais para:
  - carregar ou extrair dados do BigQuery
  - gerar mapas de clusterização com K-Means
  - plotar gráficos de foco por estado
  - minerar regras de associação climáticas usando Apriori
- `main.ipynb` - Notebook que importa o `pipeline` e executa as etapas do fluxo.
- `requirements.txt` - Dependências do projeto.
- `env example.env` - Exemplo de arquivo de variáveis de ambiente.

## Instalação

1. Crie e ative um ambiente virtual Python.
2. Instale as dependências:

```bash
pip install -r requirements.txt
```

## Variáveis de ambiente

O projeto usa `python-dotenv` para carregar o ID do projeto do Google Cloud.

Para ter acesso ao big query, acesse https://basedosdados.org/dataset/f06f3cdc-b539-409b-b311-1ff8878fb8d9?table=a3696dc2-4dd1-4f7e-9769-6aa16a1556b8

O site do basedosdados tem tutoriais explicando como criar sua conta no Google Cloud e acessar o banco de dados via Big Query

Defina em `env.env`:

```env
MEU_PROJETO_ID='seu-id-de-projeto'
```

Você pode copiar o arquivo de exemplo:

```bash
copy "env example.env" "env.env"
```

> Mantenha `env.env` fora do controle de versão.

## Uso

### Executar via notebook
1. Abra `main.ipynb` no Jupyter ou no VS Code.
2. Execute as células para carregar dados, gerar mapas e plotar gráficos.

### Executar via script
Se quiser usar diretamente o `pipeline`, abra um ambiente Python e execute:

```python
import pipeline

df = pipeline.obter_dados()
pipeline.gerar_mapas_kmeans(df)
pipeline.plotar_graficos(df)
regras = pipeline.minerar_regras_clima_bioma(df)
```

## Saída esperada

- Arquivo de cache: `queimadas_2023_2025.parquet`
- Mapas interativos: `mapa_kmeans_2023.html`, `mapa_kmeans_2024.html`, `mapa_kmeans_2025.html`
- Visualizações de gráficos no notebook ou em ambiente gráfico Python
- DataFrame de regras de associação com suporte, confiança e lift

## Observações

- O projeto depende de acesso ao BigQuery via `basedosdados`.
- Caso o Parquet local já exista, os dados serão carregados a partir do cache em vez de baixar novamente.
- Ajuste os parâmetros de `config.py` e `pipeline.minerar_regras_clima_bioma` conforme necessário.
