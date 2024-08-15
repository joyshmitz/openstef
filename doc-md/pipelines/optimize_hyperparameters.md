# openstef.pipeline.optimize_hyperparameters module

**openstef.pipeline.optimize_hyperparameters.optimize_hyperparameters_pipeline**(*pj, input_data, mlflow_tracking_uri, artifact_folder, n_trials=100*)

Оптимізація трубопроводу гіперпараметрів.

Очікувані ключі роботи прогнозування: “name”, “model”

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **input_data** (`DataFrame`) – Вхідні дані для навчання
- **mlflow_tracking_uri** (`str`) – Шлях/Uri до сервісу mlflow
- **artifact_folder** (`str`) – Шлях, де зберігаються артефакти, такі як навчені моделі
- **horizons** – Горизонти для функціональної розробки.
- **n_trials** (`int`) – Кількість випробувань. За замовчуванням N_TRIALS.

**Raises**:

- **ValueError** – Якщо вхідних даних не вистачає.
- **InputDataInsufficientError** – Якщо вхідний фрейм даних порожній.
- **InputDataWrongColumnOrderError** – Якщо стовпець навантаження відсутній у вхідному фреймі даних.
- **OldModelHigherScoreError** – Коли стара модель краща за нову.

**Return type**:

`dict`

**Returns**:

Оптимізовані гіперпараметри.

**openstef.pipeline.optimize_hyperparameters.optimize_hyperparameters_pipeline_core**(*pj, input_data, horizons=[0.25, 47.0], n_trials=100*)

Кореневий трубопровід Оптимізації гіперпараметрів.

Очікувані ключі роботи прогнозування: “name”, “model”

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **input_data** (`DataFrame`) – Вхідні дані для навчання
- **horizons** (`list`[`float`]) – horizons for feature engineering in hours.
- **n_trials** (`int`) – Кількість випробувань. За замовчуванням N_TRIALS.

**Raises**:

- **ValueError** – Якщо вхідних даних не вистачає.
- **InputDataInsufficientError** – Якщо вхідний фрейм даних порожній.
- **InputDataWrongColumnOrderError** – Якщо стовпець навантаження відсутній у вхідному фреймі даних.
- **OldModelHigherScoreError** – Коли стара модель краща за нову.
- **InputDataOngoingZeroFlatlinerError** – Коли всі останні вимірювання навантаження дорівнюють нулю.

**Return type**:

`tuple`[`OpenstfRegressor`, `ModelSpecificationDataClass`, `Report`, `dict`, `int`, `dict`[`str`, `Any`]]

**Returns**:

- Best model (найкраща модель),
- Model specifications of the best model (параметри найкращої моделі),
- Report of the best training round (звіт найкращого навчального круга),
- Trials (спроби),
- Best trial number (номер найкращої спроби),
- Optimized hyperparameters (Оптимізовані гіперпараметри).

**openstef.pipeline.optimize_hyperparameters.optuna_optimization**(*pj, objective, validated_data_with_features, n_trials*)

Виконує оптимізацію гіперпараметрів за допомогою optuna.

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **objective** (`RegressorObjective`) – Цільова функція для optuna
- **validated_data_with_features** (`DataFrame`) – очищений вхідний фрейм даних
- **n_trials** (`int`) – количество испытаний optuna

**Return type**:

`tuple`[`Study`, `RegressorObjective`]

**Returns**:

- Оптимізаційне дослідження від optuna
- Об'єкт, який використовує optuna