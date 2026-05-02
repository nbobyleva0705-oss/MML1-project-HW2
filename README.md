# B2B Product Recommender — MML1 HW2

**Autor:** Natalia Bobyleva  
**Projekt:** B2B doporučovací systém produktů (Lola Games)

## Popis projektu

Doporučovací systém pro B2B zákazníky intimního zboží. Cílem je předpovědět, které produkty zákazník objedná v příštím kvartálu, a seřadit je od nejrelevantnějšího.

- **Jednotka pozorování:** zákazník × produkt × kvartál
- **Target:** `target_ordered` (1 = objednáno, 0 = ne)
- **Metrika:** HitRate@10, Precision@10, Recall@10

## Struktura repozitáře

```
├── README.md
├── dataprocessing.ipynb        # Hlavní notebook: popis dat, split, preprocessing
├── dataprocessing.html         # HTML export (záloha)
├── benchmark.ipynb             # Benchmark notebook: baselines a ML model
├── benchmark.html              # HTML export (záloha)
├── data/
│   ├── train/
│   │   ├── train_raw.csv           # Q1+Q2, originální hodnoty
│   │   └── train_processed.csv     # Q1+Q2, po encoding a scaling
│   ├── validation/
│   │   ├── validation_raw.csv      # Q3, originální hodnoty
│   │   └── validation_processed.csv
│   └── test/
│       ├── test_raw.csv            # Q4, originální hodnoty (nepoužito v HW2)
│       └── test_processed.csv
└── b2b_recommender_tables_csv/     # Zdrojová data
    ├── customers.csv
    ├── products.csv
    ├── orders.csv
    └── training_table.csv
```

## Split

Temporální split podle kvartálu (náhodný split by způsobil data leakage):

| Sada | Kvartál(y) | Řádků |
|------|-----------|-------|
| Train | 2025Q1 + 2025Q2 | 80 300 |
| Validation | 2025Q3 | 26 300 |
| Test | 2025Q4 | 20 800 |

Test sada se v HW2 nepoužívá — zůstává stranou pro finální evaluaci.

## Jak spustit

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook dataprocessing.ipynb
jupyter notebook benchmark.ipynb
```

Notebooky jsou spustitelné od začátku do konce; všechny cesty jsou relativní.

## Výsledky benchmarků (validační sada, K=10)

| Model | HitRate@10 | Precision@10 | Recall@10 |
|-------|-----------|-------------|---------|
| Popularity Baseline | 0.700 | 0.146 | 0.116 |
| Popularity within Segment | 0.700 | 0.146 | 0.116 |
| Logistic Regression | **0.750** | **0.181** | **0.145** |
