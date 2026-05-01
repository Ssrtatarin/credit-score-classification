# Credit Score Classification

Pet-проект по классификации кредитного рейтинга клиента на основе демографических и финансовых признаков.  

## Описание проекта

В ходе проекта были выполнены следующие пункты:

- первичный анализ данных;
- проверка пропусков;
- анализ числовых и категориальных признаков;
- кодирование категориальных переменных;
- анализ дисбаланса классов;
- балансировка классов с помощью SMOTE;
- масштабирование признаков через StandardScaler;
- обучение и сравнение нескольких моделей;
- оценка качества через K-Fold Cross-Validation;
- отдельная проверка KNN-модели.

## Датасет

Датасет содержит **164 строки** и **8 признаков**.

### Признаки

| Признак | Описание |
|---|---|
| `Age` | Возраст клиента |
| `Gender` | Пол клиента |
| `Income` | Доход клиента |
| `Education` | Уровень образования |
| `Marital Status` | Семейное положение |
| `Number of Children` | Количество детей |
| `Home Ownership` | Владение жильём |
| `Credit Score` | Целевая переменная: кредитный рейтинг |

### Целевая переменная

`Credit Score` имеет три класса:

| Класс | Значение |
|---|---|
| `Low` | Низкий кредитный рейтинг |
| `Average` | Средний кредитный рейтинг |
| `High` | Высокий кредитный рейтинг |

## Предобработка данных

### Ordinal Encoding

Для признаков с очевидным порядком используется `OrdinalEncoder`:

- `Education`
- `Credit Score`

Порядок для `Education`:

```text
High School Diploma < Associate's Degree < Bachelor's Degree < Master's Degree < Doctorate
```

Порядок для `Credit Score`:

```text
Low < Average < High
```

### One-Hot Encoding

Для остальных категориальных признаков используется `OneHotEncoder`:

- `Gender`
- `Marital Status`
- `Home Ownership`


## Работа с дисбалансом классов

После анализа распределения таргета, я выявил дисбаланс классов.  
Для балансировки используется метод **SMOTE**, а также масштабировал признаки.
Масштабирование важно для моделей, чувствительных к расстояниям и масштабу признаков, такие как KNN и Logistic Regression.

## Модели

В проекте сравниваются следующие модели:

- `RandomForestClassifier`
- `LogisticRegression`
- `XGBClassifier`
- `KNeighborsClassifier`

## Метрики качества

Для оценки моделей используются следующие метрики:

- `accuracy`
- `precision_macro`
- `recall_macro`
- `f1_macro`
- `roc_auc_ovr`

`roc_auc_ovr` используется для многоклассовой классификации по стратегии **One-vs-Rest**.

## Результаты моделей

### Cross-Validation Results

| Model | Accuracy | Precision Macro | Recall Macro | F1 Macro | ROC-AUC OvR |
|---|---:|---:|---:|---:|---:|
| Random Forest | 0.9971 | 0.9956 | 0.9968 | 0.9961 | 0.9977 |
| Logistic Regression | 0.9705 | 0.9700 | 0.9697 | 0.9692 | 0.9971 |
| XGBClassifier | 0.9882 | 0.9891 | 0.9854 | 0.9870 | 0.9963 |
| KNN | 0.9824 | 0.9859 | 0.9817 | 0.9817 | 0.9930 |

Лучший результат по основным метрикам показала модель **RandomForestClassifier**.

## KNN Evaluation

Для KNN я отедльно разделил датасет на train/test split ради эксеримента.

Accuracy: 1.0

Classification report:
              precision    recall  f1-score   support

         0.0       1.00      1.00      1.00        36
         1.0       1.00      1.00      1.00        40
         2.0       1.00      1.00      1.00        26

    accuracy                           1.00       102
   macro avg       1.00      1.00      1.00       102
weighted avg       1.00      1.00      1.00       102
```

### Confusion Matrix

```text
[[64  0  0]
 [ 1 54  5]
 [ 0  1 45]]
```


## Структура проекта

```text
credit-score-classification/
│
├── data/
│   └── credit_dataset.csv
│
├── model.ipynb
├── README.md
└── requirements.txt
```



## Вывод
Лучше всего на фолдах показал себя RandomForest. Отедельный эксперимент для KNN показал по всем метрикам идеальный результат.

