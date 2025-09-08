# 📊 Highway [VisioAi]: Índice de Saúde Financeira das Lojas HighWay  

## 🛠️ Objetivo  
Este projeto trata os dados brutos e cria um **índice de saúde financeira** das lojas a partir de dados de diversas tabelas disponibilizadas em formato `.CSV` e colocadas no **Google BigQuery**.  

O índice varia de **0 a 10** e combina métricas como:  
- **Faturamento total**  
- **Torque médio** (delivery e em loja)  
- **Descontos aplicados**  
- **Quantidade de itens vendidos**  

Este índice  alimenta um **dashboard no Looker Studio** que:  
- Gera um **ranking de desempenho financeiro** das lojas.  
- Identifica **situações de alerta** e **variáveis críticas** a melhorar.  
- Fornece uma visão **simples e intuitiva** para gestores não técnicos (feito com a intenção de bater o olho e obter respostas dos pontos-chave).  

(Fluxo dos dados)
CSV → BigQuery → Python ETL → Tabelas Finais → Looker Studio Dashboard

---

## 🛠️ Ferramentas utilizadas  

### 1. **BigQuery / SQL**  
- Hospeda os dados brutos.  
- Utilizado para integração com Python e Looker Studio.  

### 2. **Python**  
Usado para todo o **ETL**.  

Bibliotecas principais:  
- **google-cloud-bigquery** -> Conexão com BigQuery.  
- **pandas** -> Tratamento, Manipulação e padronização dos dados (limpeza, agregações, cálculos do índice e etc...).  

### 3. **Looker Studio**  
Usado para **Visualização e Análise Interativa**.  

Exibições principais:  
- **Índice de Saúde Geral** (Valores agrupados por loja).  
- **Índice de Saúde Mensal** (Valores agrupados por período (mensal)).  
- **Tabela Detalhada** (Tabela geral com que permite ir a fundo nos dados e filtrar conforme necessidade).  

---

## 🔄 Como reproduzir o projeto

### 1. Clone o Repositório
```bash
git clone https://github.com/BrCOS/Highway_VisioAi_Indice_Saude.git
cd Highway_VisioAi (crie a pasta que preferir)
```

### 2. Crie um Ambiente Virtual Python
```bash
python -m venv venv
venv\Scripts\activate
source venv/bin/activate (para Mac e Linux)
```

### 3. Instale os Requisitos
```bash
pip install -r requirements.txt
```

### 4. Crie e configure o arquivo `.env`
```bash
Preencha ele com o caminho do JSON e informações dos bancos e tabelas necessárias.
```

### 5. Crie (e baixe) uma chave de acesso .JSON no Google BigQuery
```bash
Crie uma chave de acesso no Google Cloud.
Baixe o arquivo .json da chave de serviço.
Coloque dentro do repositório na pasta /JSON.
```

### 6. Execute o notebook main.ipynb
Este script (ETL), vai carregar os dados (de diversas tabelas) do banco, criar alguns campos, unificar as bases com base no identificador, remover campos desnecessários e salvar em formato `.CSV`.

### 7. Execute o notebook indice_loja.ipynb
Este script (ETL), vai carregar os dados tratados do banco (única tabela), padronizar e calcular os índices de saúde em duas formas: Geral e Mensal e salvar em formato `.CSV`.

### 8. Verifique oLooker Studio
Conecte os dados do BigQQuery no Looker Studio e divirta-se.