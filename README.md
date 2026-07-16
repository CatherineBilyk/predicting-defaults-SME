# Predicting Defaults SME

Оценка вероятности дефолта (PD) для малого и среднего бизнеса (МСП) на основе финансовой отчётности компаний и макроэкономических показателей.

## О проекте

Цель — построить панельную модель раннего предупреждения о банкротстве компании по её финансовым коэффициентам и сравнить два подхода:

- **Logistic Regression** (pooled logit, с пошаговым отбором признаков и топ-10 рискованных категорий по региону/отрасли)
- **CatBoost** (градиентный бустинг с весами классов под сильный дисбаланс)

Данные — панель "компания × год" за 2015–2024 гг., собранная из выгрузок СПАРК (финансовые показатели ликвидированных и действующих компаний) и дополненная макропоказателями (ключевая ставка, курс USD/RUB, ВРП на душу населения по регионам).

## Данные

- **Объём выборки:** 31 996 наблюдений (панель компания-год), 25 признаков
- **Доля дефолтов:** ~3.3% (сильный дисбаланс классов)
- **Целевая переменная:** `next_year_liqudation` — ликвидация компании в следующем году
- **Признаки:** коэффициенты ликвидности, оборачиваемости, рентабельности, долговой нагрузки + макропоказатели (ключевая ставка, USD/RUB) + регион/отрасль

## Структура репозитория

```
├── data/
│   ├── raw/                 # исходные выгрузки СПАРК (.xlsx) и макроданные
│   └── processed/           # очищенные и объединённые датасеты (healthy + bancrupted)
├── notebooks/
│   ├── healthy_dataset.ipynb    # сборка и очистка выборки действующих компаний
│   ├── bancrupted_dataset.ipynb # сборка и очистка выборки ликвидированных компаний
│   ├── macro_data.ipynb         # подготовка и присоединение макропоказателей
│   ├── supid_logit.ipynb        # pooled логистическая регрессия (2 варианта)
│   └── model.ipynb              # CatBoost, оценка качества, SHAP, early warning
├── defaults.pdf              # итоговый отчёт по проекту
└── README.md
```

## Как воспроизвести

```bash
git clone https://github.com/CatherineBilyk/predicting-defaults-SME.git
cd predicting-defaults-SME
pip install pandas numpy scikit-learn catboost shap imbalanced-learn matplotlib seaborn scipy
```

Порядок запуска ноутбуков:
1. `healthy_dataset.ipynb` и `bancrupted_dataset.ipynb` — сборка панели из сырых выгрузок
2. `macro_data.ipynb` — присоединение макропоказателей
3. `supid_logit.ipynb` — baseline-модель (логистическая регрессия)
4. `model.ipynb` — основная модель (CatBoost), сравнение метрик, интерпретация (SHAP)

## Стек

Python: pandas, numpy, scikit-learn, catboost, shap, imbalanced-learn, matplotlib, seaborn, scipy

## Автор

Bilyk Ekaterina — [GitHub](https://github.com/CatherineBilyk)
