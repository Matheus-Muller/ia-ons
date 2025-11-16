# 🔌 IA-ONS: Inteligência Artificial para o Setor Elétrico

Aplicação completa de técnicas de Inteligência Artificial para análise e previsão de dados do Operador Nacional do Sistema Elétrico (ONS), abrangendo todo o pipeline de ciência de dados: desde ETL e análise exploratória até modelos de Deep Learning e sistemas RAG com LLMs.

---

## 📚 Índice

- [Visão Geral](#-visão-geral)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Notebooks](#-notebooks)
  - [1. ETL e EDA](#1-etl-e-eda)
  - [2. Aprendizado Supervisionado e Não Supervisionado](#2-aprendizado-supervisionado-e-não-supervisionado)
  - [3. Deep Learning com LSTM](#3-deep-learning-com-lstm)
  - [4. Sistema RAG com LLM](#4-sistema-rag-com-llm)
- [Instalação](#-instalação)
- [Resultados e Aplicações](#-resultados-e-aplicações)

---

## 🎯 Visão Geral

Este projeto demonstra a aplicação de técnicas de IA no contexto do setor elétrico brasileiro, utilizando dados reais do ONS. O trabalho foi desenvolvido com foco em:

- **Análise de Dados**: ETL e exploração de dados históricos de carga elétrica
- **Machine Learning Clássico**: Modelos de regressão e clustering para previsão e segmentação
- **Deep Learning**: Redes LSTM para previsão de séries temporais
- **LLMs e RAG**: Sistema de perguntas e respostas sobre procedimentos de rede do ONS

---

## 📁 Estrutura do Projeto

```
ia-ons/
├── 1-ETL_EDA.ipynb                      # ETL e Análise Exploratória
├── 2-supervised_unsupervised_learning.ipynb  # Machine Learning
├── 3-deep_learning_lstm.ipynb           # Deep Learning
├── 4-RAG.ipynb                          # Sistema RAG com LLM
├── requirements.txt                     # Dependências do projeto
├── README.md                           # Este arquivo
└── db/
    ├── dataset.csv                     # Dataset processado
    ├── chroma_db/                      # Vector store para RAG
    └── rag_context/                    # PDFs dos procedimentos ONS
```

---

## 📓 Notebooks

### 1. ETL e EDA
**📂 Notebook:** [`1-ETL_EDA.ipynb`](./1-ETL_EDA.ipynb)

#### 🎯 Objetivos
- Realizar ETL (Extract, Transform, Load) dos dados do ONS
- Executar análise exploratória completa
- Preparar dados para modelagem

#### 🔧 Funcionalidades
- **Extração e limpeza de dados** históricos de carga elétrica
- **Análise temporal**: Tendências, sazonalidade e padrões
- **Visualizações**:
  - Série temporal completa
  - Análise de sazonalidade (diária, semanal, mensal, anual)
  - Distribuições e estatísticas descritivas
  - Matriz de correlação
  - Análise de outliers
  - Padrões por hora do dia e dia da semana

#### 📊 Principais Descobertas
- Padrões claros de consumo diário e semanal
- Sazonalidade anual relacionada a fatores climáticos
- Identificação de períodos de pico e vales de consumo
- Correlações entre variáveis temporais e carga

---

### 2. Aprendizado Supervisionado e Não Supervisionado
**📂 Notebook:** [`2-supervised_unsupervised_learning.ipynb`](./2-supervised_unsupervised_learning.ipynb)

#### 🎯 Objetivos
- Aplicar técnicas de aprendizado supervisionado para previsão
- Utilizar aprendizado não supervisionado para segmentação
- Comparar diferentes algoritmos de ML

#### 🔧 Funcionalidades

##### 🎓 Aprendizado Supervisionado (Regressão)
- **Modelos implementados**:
  - Random Forest Regressor
  - Gradient Boosting Regressor
  - XGBoost
- **Feature Engineering**:
  - Features temporais (hora, dia da semana, mês)
  - Features de lag (valores anteriores)
  - Médias móveis
- **Avaliação**:
  - RMSE, MAE, R², MAPE
  - Análise de resíduos
  - Importância de features

##### 🔍 Aprendizado Não Supervisionado (Clustering)
- **Algoritmo**: K-Means
- **Segmentação de padrões** de consumo
- **Análise de clusters**:
  - Perfis de carga por cluster
  - Características temporais

---

### 3. Deep Learning com LSTM
**📂 Notebook:** [`3-deep_learning_lstm.ipynb`](./3-deep_learning_lstm.ipynb)

#### 🎯 Objetivos
- Implementar rede neural LSTM para previsão de séries temporais
- Capturar dependências temporais de longo prazo
- Realizar previsões multi-step

#### 🔧 Funcionalidades

##### 🧠 Arquitetura do Modelo
- **Tipo**: Bidirectional LSTM
- **Camadas**:
  - Bidirectional LSTM (128 unidades)
  - LSTM (64 unidades)
  - LSTM (32 unidades)
  - Dense (16 unidades)
  - Output (1 unidade)
- **Regularização**: Dropout (0.2)
- **Otimizador**: Adam
- **Loss**: MSE

##### 📈 Preparação dos Dados
- **Normalização**: MinMaxScaler [0, 1]
- **Janela temporal**: 168 horas (1 semana)
- **Divisão**:
  - Treino: 70%
  - Validação: 15%
  - Teste: 15%

##### 🎯 Treinamento
- **Callbacks**:
  - Early Stopping (patience=15)
  - Model Checkpoint
  - Reduce Learning Rate on Plateau
- **Batch Size**: 64
- **Épocas**: 30 (com early stopping)

---

### 4. Sistema RAG com LLM
**📂 Notebook:** [`4-RAG.ipynb`](./4-RAG.ipynb)

#### 🎯 Objetivos
- Implementar sistema RAG (Retrieval-Augmented Generation)
- Integrar LLM com base de conhecimento específica do domínio

#### 🔧 Funcionalidades

##### 📚 Processamento de Documentos
- **Fonte**: 16 PDFs de Procedimentos de Rede do ONS
- **Extração**: PyPDF2
- **Chunking**: 
  - Tamanho: 1000 caracteres
  - Overlap: 200 caracteres
  - Splitter: RecursiveCharacterTextSplitter

##### 🔍 Retrieval (Busca Semântica)
- **Embeddings**: all-MiniLM-L6-v2 (multilíngue)
- **Vector Store**: ChromaDB
- **Dimensão**: 384
- **Busca**: Top-k similarity search

##### 🤖 Generation (LLM)
- **Modelo**: Google Gemini 2.5 Flash
- **Capacidades**:
  - Respostas contextualizadas
  - Citação de fontes
  - Respostas em português
  - Limitação ao contexto fornecido

##### 💬 Sistema Completo
- **Pipeline RAG**:
  1. Usuário faz pergunta
  2. Sistema busca documentos relevantes
  3. Contexto é montado
  4. LLM gera resposta baseada no contexto
  5. Resposta inclui citações das fontes

#### 📊 Exemplos de Uso
```python
# Fazer pergunta ao sistema
result = rag_query(
    question="O que é feito na Programação Diária da Operação?",
    k=3,
    verbose=True
)
```

#### 🎯 Aplicações
- Consulta rápida a procedimentos operacionais
- Treinamento de operadores
- Auditoria e conformidade
- Suporte à decisão operacional

---

## 🚀 Instalação

### Pré-requisitos
- Python 3.8+
- Jupyter Notebook ou VS Code com extensão Python

### Passos

1. **Clone o repositório**
```bash
git clone https://github.com/seu-usuario/ia-ons.git
cd ia-ons
```

2. **Instale as dependências**
```bash
pip install -r requirements.txt
```

3. **Configure a API do Gemini** (para o notebook 4)
   - Obtenha uma chave gratuita em: https://makersuite.google.com/app/apikey
   - Adicione sua chave no notebook `4-RAG.ipynb`

4. **Execute os notebooks na ordem**
   - Comece pelo `1-ETL_EDA.ipynb`
   - Prossiga sequencialmente pelos demais

---

## 👨‍💻 Autor

Matheus Müller

