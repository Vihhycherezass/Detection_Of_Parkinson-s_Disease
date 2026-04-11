# Диагностика болезни Паркинсона по голосовым признакам

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![XGBoost](https://img.shields.io/badge/XGBoost-1.5+-orange.svg)
![Status](https://img.shields.io/badge/status-completed-brightgreen.svg)
![Accuracy](https://img.shields.io/badge/accuracy-97.44%25-success.svg)

Pet-проект по бинарной классификации пациентов на здоровых и больных болезнью Паркинсона на основе 22 голосовых признаков. Используются XGBoost, Random Forest и SVM в ансамбле. Достигнута точность **97.44%** на тестовой выборке.

---

## Содержание
- [Использование](#использование)
- [Разработка](#разработка)
- [Результаты](#результаты)
- [To do](#to-do)
- [Команда проекта](#команда-проекта)
- [Источники](#источники)

---

## Использование

Запустите Jupyter Notebook или Python-скрипт. Основные шаги:

- Загрузка датасета `parkinsons.data`.
- Удаление столбца `name`, выделение признаков и целевой переменной (`status`).
- Стандартизация признаков (StandardScaler).
- Обучение модели (XGBoost, ансамбль VotingClassifier).

---

## Разработка

Пример обучения ансамбля:

```python
from sklearn.ensemble import VotingClassifier
from xgboost import XGBClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC

xgb = XGBClassifier(n_estimators=200, max_depth=5, learning_rate=0.1)
rf = RandomForestClassifier(n_estimators=200, max_depth=5)
svm = SVC(probability=True, C=1.0, gamma='scale')

ensemble = VotingClassifier(
    estimators=[('xgb', xgb), ('rf', rf), ('svm', svm)],
    voting='soft'
)
```
Подбор гиперпараметров с RandomizedSearchCV:

```
param_grid = {
    'xgb__n_estimators': [150, 200, 250, 300],
    'xgb__max_depth': [3, 4, 5, 6],
    'xgb__learning_rate': [0.01, 0.05, 0.1, 0.15]
}
random_search = RandomizedSearchCV(pipeline, param_grid, n_iter=50, cv=5)
```

---

### Результаты

- Лучшая точность ансамбля: 97.44%
- Базовая модель XGBoost: 92.31%
- После подбора гиперпараметров (RandomizedSearchCV): 94.87%
- 
Матрица ошибок ансамбля показывает минимальное количество ложных срабатываний.

Топ-5 наиболее важных признаков (по XGBoost):
```'MDVP:Fo(Hz)', 'MDVP:Fhi(Hz)', 'MDVP:Jitter(%)', 'HNR', 'RPDE'```

---

### To do
- Базовая модель XGBoost
- Cтандартизация и стратификация
- Подбор гиперпараметров (RandomizedSearchCV)
- Ансамбль (XGBoost + RandomForest + SVM)
- Применение SMOTE для балансировки классов
- Добавить логистическую регрессию в ансамбль
----

### Команда проекта
Соло-разработчик: [Назар](https://github.com/Vihhycherezass)

----
### Источники
- Вдохновение: задача бинарной классификации с дисбалансом классов
