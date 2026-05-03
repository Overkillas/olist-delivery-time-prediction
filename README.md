# Projeto 2 — Aprendizado de Máquina: Olist

Previsão do **tempo de entrega** de pedidos do marketplace **Olist** usando um *pipeline* completo de Machine Learning (engenharia de atributos, EDA, treinamento e avaliação de modelos de regressão).

> Disciplina **T326 - Ciência dos Dados** — Professor Caio Ponte — Turma 16/17

---

## Estrutura do projeto

```
av2 - compt dist/
├── .gitignore
├── README.md                                ← este arquivo
├── requirements.txt                         ← dependências Python
├── dataset/                                 ← CSVs do Olist (não versionado)
│   ├── olist_customers_dataset.csv
│   ├── olist_geolocation_dataset.csv
│   ├── olist_order_items_dataset.csv
│   ├── olist_order_payments_dataset.csv
│   ├── olist_order_reviews_dataset.csv
│   ├── olist_orders_dataset.csv
│   ├── olist_products_dataset.csv
│   ├── olist_sellers_dataset.csv
│   └── product_category_name_translation.csv
├── import_dataset.py                        ← script de download via kagglehub
└── olist_delivery_time_prediction.ipynb     ← notebook principal (52 células)
```

---

## Pré-requisitos

- **Python 3.10+** (testado em 3.13)
- **pip** atualizado
- (Opcional) **conta no Kaggle** caso queira baixar o dataset via `kagglehub`

---

## Passo a passo para rodar

### 1. Clonar / baixar o projeto

Coloque o projeto em uma pasta local. Em seguida abra o terminal **na raiz do projeto** (a pasta que contém este `README.md`).

### 2. Criar e ativar um ambiente virtual

**Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**Windows (CMD):**
```cmd
python -m venv venv
venv\Scripts\activate.bat
```

**Linux / macOS:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Instalar as dependências

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

Isso instala: `pandas`, `numpy`, `scikit-learn`, `category_encoders`, `matplotlib`, `seaborn`, `plotly`, `jupyter`, `ipykernel`, `notebook` e `kagglehub`.

### 4. Obter os dados

Existem **duas opções**:

#### Opção A — Baixar via script (recomendado)

```bash
python import_dataset.py
```

O script usa `kagglehub` para baixar a versão mais recente do dataset `olistbr/brazilian-ecommerce` e imprime o caminho local onde os arquivos foram salvos. Copie todos os 9 CSVs para a pasta `dataset/` na raiz do projeto.

> **Atenção:** o `kagglehub` exige autenticação. Configure a chave da API do Kaggle (`~/.kaggle/kaggle.json` no Linux/macOS ou `C:\Users\<usuario>\.kaggle\kaggle.json` no Windows) antes de rodar o script. Tutorial oficial: <https://www.kaggle.com/docs/api>.

#### Opção B — Download manual

1. Acesse <https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce>
2. Faça login no Kaggle e clique em **Download**
3. Extraia o `.zip` e mova **os 9 arquivos `.csv`** para a pasta `dataset/` na raiz do projeto

Ao final, a pasta `dataset/` deve conter **exatamente** estes arquivos:

```
olist_customers_dataset.csv
olist_geolocation_dataset.csv
olist_order_items_dataset.csv
olist_order_payments_dataset.csv
olist_order_reviews_dataset.csv
olist_orders_dataset.csv
olist_products_dataset.csv
olist_sellers_dataset.csv
product_category_name_translation.csv
```

### 5. Abrir o notebook

```bash
jupyter notebook
```

ou, se preferir o Jupyter Lab:

```bash
jupyter lab
```

Na interface aberta no navegador, clique em `olist_delivery_time_prediction.ipynb`.

> Você também pode abrir o `.ipynb` diretamente no **VS Code** (extensão Jupyter) ou no **Google Colab** — basta fazer upload do notebook e dos CSVs.

### 6. Executar as células

No menu, escolha **Cell → Run All** (ou `Kernel → Restart & Run All` para garantir um *kernel* limpo).

A execução completa leva alguns minutos por causa do tuning de hiperparâmetros (`GridSearchCV` e `RandomizedSearchCV` em 4 modelos diferentes). Você verá:

1. **Setup** — imports e validação dos arquivos do dataset
2. **Seção 1** — definição do problema e schema das tabelas
3. **Seção 2** — pré-processamento (engenharia de atributos, limpeza, imputação, escalonamento, codificação)
4. **Seção 3** — EDA (distribuições, outliers, correlações, mapas geográficos)
5. **Seção 4** — treinamento e tuning de Ridge, Random Forest, Gradient Boosting e KNN
6. **Seção 5** — avaliação no conjunto de teste, benchmark e conclusão

---

## Solução de problemas

| Problema | Causa provável | Solução |
|---|---|---|
| `FileNotFoundError: Arquivos do dataset não encontrados` | CSVs não estão em `dataset/` | Verifique se os 9 arquivos estão na pasta correta com os nomes exatos |
| `ModuleNotFoundError: category_encoders` (ou outra lib) | Dependências não instaladas | Rode `pip install -r requirements.txt` com o venv ativo |
| Mapas Plotly não aparecem | *Renderer* não detectado | No Jupyter Notebook clássico instale `pip install "notebook>=7"` ou abra o notebook no VS Code/Lab |
| Tuning muito demorado | Datasets grandes + busca exaustiva | Reduza `n_iter` no `RandomizedSearchCV` ou diminua os ranges de hiperparâmetros |
| Erro de autenticação no `kagglehub` | Chave da API ausente | Gere `kaggle.json` em <https://www.kaggle.com/settings> e coloque em `~/.kaggle/` |

---

## Reprodutibilidade

- Seed global: `SEED = 42` aplicada em `numpy`, `train_test_split`, `KFold`, `RandomForestRegressor`, `GradientBoostingRegressor`, `Ridge` e `RandomizedSearchCV`.
- Pré-processamento (imputação, escala, codificação) está encapsulado em `Pipeline` + `ColumnTransformer` — **não há vazamento de dados** entre treino e teste.
- O notebook salva todas as suas saídas (gráficos, tabelas), então pode ser revisado mesmo sem reexecução.

---

## Sobre o conteúdo

Todo o conteúdo do notebook (Markdown, comentários, títulos de gráficos, prints) está em **português brasileiro**.
