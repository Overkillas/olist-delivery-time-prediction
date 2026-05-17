# Projeto 2 — Aprendizado de Máquina: Olist

Predição do **tempo de entrega** (em dias) de pedidos do marketplace **Olist** — pipeline completo de Aprendizado de Máquina cobrindo definição do problema, pré-processamento, EDA, análise de utilidade de features, treinamento com tuning e avaliação final.

> Disciplina **T326 - Ciência dos Dados** — Professor Caio Ponte — Turma 16/17

---

## Resumo dos resultados

| Modelo final | RMSE | MAE | R² |
|---|---|---|---|
| **HistGradientBoosting** | **5.87 dias** | **4.16 dias** | **0.435** |

Bate o baseline ingênuo (`DummyRegressor`) em **1.85 dia de RMSE**. Foram treinados 4 modelos de 3 famílias diferentes (Ridge, Decision Tree, Random Forest, HistGradientBoosting), todos tunados com `GridSearchCV` / `RandomizedSearchCV` em validação cruzada de 5 folds. Detalhes na Seção 6 do notebook.

---

## Estrutura do projeto

```
av2 - compt dist/
├── .gitignore
├── README.md                                ← este arquivo
├── requirements.txt                         ← dependências Python (versões travadas)
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
├── docs/
│   └── Projeto 2 - Aprendizado de Máquina_ Olist.pdf  ← enunciado oficial
├── import_dataset.py                        ← script de download via kagglehub
└── olist_delivery_time_prediction.ipynb     ← notebook principal (79 células, 6 seções)
```

---

## Estrutura do notebook

| Seção | Conteúdo | Peso no PDF |
|---|---|---|
| **1. Definição do Problema** | Contexto Olist, definição da regressão, schema das tabelas | 10% |
| **2. Pré-processamento** | Engenharia de atributos, limpeza, imputação, escalonamento, codificação | 30% |
| **3. EDA** | Distribuições, outliers, correlações, descritivas, mapas geográficos | 10% |
| **4. Análise da Utilidade das Features** *(extra)* | 6 métodos de feature importance + ranking consolidado | — |
| **5. Treinamento do Modelo** | 4 modelos (Ridge, Tree, RF, HGB) com tuning Grid/Randomized | 30% |
| **6. Avaliação e Conclusão** | RMSE/MAE/R², benchmark, diagnósticos, melhorias, conclusão | 20% |

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

Isso instala (versões travadas): `pandas`, `numpy`, `scipy`, `scikit-learn`, `category_encoders`, `matplotlib`, `seaborn`, `plotly`, `jupyter`, `jupyterlab`, `ipykernel`, `nbconvert`, `kagglehub` e suas transitivas.

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

O notebook **já vem com todos os outputs salvos** (tabelas, gráficos, métricas), então você pode apenas revisar sem reexecutar.

Se quiser reexecutar do zero:
- **Cell → Run All** ou **Kernel → Restart & Run All**.
- A execução completa demora **~15-25 minutos**, dominada pelo tuning do Random Forest (~15 min) e do HistGradientBoosting (~4 min) na Seção 5.
- Se quiser uma execução rápida (sem tuning), pule as células das Seções 5.7 e 5.8 que rodam `RandomizedSearchCV`.

---

## Solução de problemas

| Problema | Causa provável | Solução |
|---|---|---|
| `FileNotFoundError: Arquivos do dataset não encontrados` | CSVs não estão em `dataset/` | Verifique se os 9 arquivos estão na pasta correta com os nomes exatos |
| `ModuleNotFoundError: category_encoders` (ou outra lib) | Dependências não instaladas | Rode `pip install -r requirements.txt` com o venv ativo |
| Mapas Plotly não aparecem | *Renderer* não detectado | No Jupyter Notebook clássico instale `pip install "notebook>=7"` ou abra o notebook no VS Code/Lab |
| Erro de autenticação no `kagglehub` | Chave da API ausente | Gere `kaggle.json` em <https://www.kaggle.com/settings> e coloque em `~/.kaggle/` |
| Tuning demora muito | `RandomizedSearchCV` com `n_iter=12` em RF e HGB | Reduzir `n_iter` para 5 ou pular as células de tuning pesado |

---

## Reprodutibilidade

- **Seed global:** `SEED = 42` aplicada em `numpy`, `train_test_split`, `KFold`, `RandomForestRegressor`, `HistGradientBoostingRegressor`, `RandomizedSearchCV` e `permutation_importance`.
- **Pipelines:** todo o pré-processamento (imputação, escala, codificação) é encapsulado em `Pipeline` + `ColumnTransformer`, garantindo que treino, validação cruzada e teste passem pelas mesmas transformações sem vazamento de dados.
- **Versões travadas** em `requirements.txt` para evitar quebras de API entre versões.
- O notebook salva todas as suas saídas (gráficos, tabelas, métricas), então pode ser revisado mesmo sem reexecução.

---

## Sobre o conteúdo

Todo o conteúdo do notebook (Markdown, comentários, títulos de gráficos, prints) está em **português brasileiro**.
