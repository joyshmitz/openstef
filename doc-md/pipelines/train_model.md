# openstef.pipeline.train_model module
**openstef.pipeline.train_model.train_model_pipeline**(*pj, input_data, check_old_model_age, mlflow_tracking_uri, artifact_folder*)

Трубопровід середнього рівня, який піклується про всі постійні залежності від сховища.

Очікувані ключі робіт прогнозування: “id”, “model”, “hyper_params”, “feature_names”.

**Parameters**:

- **pj** ([`PredictionJobDataClass`](https://openstef.github.io/openstef/openstef.data_classes.html#openstef.data_classes.prediction_job.PredictionJobDataClass)) – Робота прогнозування
- **input_data** (`DataFrame`) – Вхідні дані для навчання
- **check_old_model_age** (`bool`) – Перевіряє, чи не варто пропустити навчання через те, що модель занадто молода
- **mlflow_tracking_uri** (`str`) – Відстежує URI для MLFlow
- **artifact_folder** (`str`) – Шлях, де зберігаються артефакти, такі як навчені моделі

**Return type**:

`Optional`[`tuple`[`DataFrame`, `DataFrame`, `DataFrame`]]

**Returns**:

Якщо pj.save_train_forecasts приймає значення False, ***None*** повертається, в іншому випадку:

> - Тренувальний набір даних з прогнозами
> - Перевірочний набір даних з прогнозами
> - Тестовий набір даних з прогнозами

**Raises**:

- [**InputDataInsufficientError**](https://openstef.github.io/openstef/openstef.html#openstef.exceptions.InputDataInsufficientError) – коли вхідних даних недостатньо.
- [**InputDataWrongColumnOrderError**](https://openstef.github.io/openstef/openstef.html#openstef.exceptions.InputDataWrongColumnOrderError) – коли вхідні дані мають неправильний порядок стовпців. стовпець 'load' має бути першим, а стовпець 'horizon' останнім.
- [**OldModelHigherScoreError**](https://openstef.github.io/openstef/openstef.html#openstef.exceptions.OldModelHigherScoreError) – Коли стара модель краща за нову.
- [**SkipSaveTrainingForecasts**](https://openstef.github.io/openstef/openstef.html#openstef.exceptions.SkipSaveTrainingForecasts) – Якщо стара модель краща або молодша за MAXIMUM_MODEL_AGE, модель не зберігається.

**openstef.pipeline.train_model.train_model_pipeline_core***(*pj, model_specs, input_data, old_model=None, horizons=[0.25, 47.0]*)

Трубопровід кореневої тренувальної моделі.

Навчає нову модель на основі роботи прогнозування, вхідних даних і порівнює її зі старою моделлю. Цей трубопровід не має бази даних або постійних залежностей від сховища.

**Parameters**:

- **pj** ([`PredictionJobDataClass`](https://openstef.github.io/openstef/openstef.data_classes.html#openstef.data_classes.prediction_job.PredictionJobDataClass)) – Робота прогнозування
- **model_specs** ([`ModelSpecificationDataClass`](https://openstef.github.io/openstef/openstef.data_classes.html#openstef.data_classes.model_specifications.ModelSpecificationDataClass)) – Клас даних, що містить характеристики моделі
- **input_data** (`DataFrame`) – Вхідні дані
- **old_model** (`Optional`[[`OpenstfRegressor`](https://openstef.github.io/openstef/openstef.model.regressors.html#openstef.model.regressors.regressor.OpenstfRegressor)]) – Стара модель для порівняння. За замовчуванням - Ні.
- **horizons** (`list`[`float`]) – Горизонти для тренувань в ті години, що відповідають розробці функціоналу.

**Raises**:

- **InputDataInsufficientError** – коли вхідних даних недостатньо.
- **InputDataWrongColumnOrderError** – коли вхідні дані мають неправильний порядок стовпців.
- **OldModelHigherScoreError** – коли стара модель краща за нову.
- [**InputDataOngoingZeroFlatlinerError**](https://openstef.github.io/openstef/openstef.html#openstef.exceptions.InputDataOngoingZeroFlatlinerError) – коли всі останні вимірювання навантаження дорівнюють нулю.

**Return type**:

`Tuple` [`OpenstfRegressor`, [`Report`](https://openstef.github.io/openstef/openstef.metrics.html#openstef.metrics.reporter.Report), `ModelSpecificationDataClass`, `tuple`[`DataFrame`, `DataFrame`, `DataFrame`]]

**Returns**:

- Fitted_model (OpenstfRegressor)
- Report (Звіт)
- Modelspecs (ModelSpecificationDataClass)
- Datasets (tuple[pd.DataFrmae, pd.DataFrame, pd.Dataframe]): Тренувальні, перевірочні та тестові набори

**openstef.pipeline.train_model.train_pipeline_common**(*pj, model_specs, input_data, horizons, test_fraction=0.0, backtest=False, test_data_predefined=Empty DataFrame Columns: [] Index: []*)

Спільний трубопровід для оперативного навчання та навчання з бек-тестування.

**Parameters**:
- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **model_specs** (`ModelSpecificationDataClass`) – Клас даних, що містить характеристики моделі
- **input_data** (`DataFrame`) – Вхідні дані
- **horizons** (`list`[`float`]) – Горизонти для тренувань в годинах.
- **test_fraction** (`float`) – частина даних для тестування
- **backtest** (`bool`) – логічна величина, якщо нам потрібно зробити бектест
- **test_data_predefined** (`DataFrame`) – Попередньо визначений фрейм даних тесту, який буде використовуватися в трубопроводі (за замовчуванням порожній фрейм даних)

**Return type**:

`tuple`[`OpenstfRegressor`, `Report`, `DataFrame`, `DataFrame`, `DataFrame`, `DataFrame`]

**Returns**:

- The trained model (модель, що тренувалась)
- Report (Звіт)
- The train data (навчальні дані)
- The validation data (перевірочні дані)
- The test data (тестувальні дані)

**Raises**:

- **InputDataInsufficientError** – коли вхідних даних недостатньо.
- **InputDataWrongColumnOrderError** – коли вхідні дані мають неправильний порядок стовпців. стовпець 'load' має бути першим, а стовпець 'horizon' останнім.
- **InputDataOngoingZeroFlatlinerError** – коли всі останні вимірювання навантаження дорівнюють нулю.

**openstef.pipeline.train_model.train_pipeline_step_compute_features**(*pj, model_specs, input_data, horizons=list[float]*)

Обчислення функцій і перевірка відповідності.

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **model_specs** (`ModelSpecificationDataClass`) – Клас даних, що містить характеристики моделі
- **input_data** (`DataFrame`) – Вхідні дані
- **horizons** – Горизонти для тренувань в годинах.

**Return type**:

`DataFrame`

**Returns**:

Фрейм даних з функціями, необхідними для навчання моделі

**Raises**:

- **InputDataInsufficientError** –коли вхідних даних недостатньо.
- **InputDataWrongColumnOrderError** – коли вхідні дані мають неправильний порядок стовпців.
- **ValueError** – коли горизонт є рядком і відповідного стовпчика немає у вхідних даних
- **InputDataOngoingZeroFlatlinerError** – коли всі останні вимірювання навантаження дорівнюють нулю.

**openstef.pipeline.train_model.train_pipeline_step_load_model**(*pj, serializer*)

**Return type**:

`Tuple`[`OpenstfRegresso`r, `ModelSpecificationDataClass`, `Union`[`int`, `float`]]

**openstef.pipeline.train_model.train_pipeline_step_split_data**(*data_with_features, pj, test_fraction, backtest=False, test_data_predefined=Empty DataFrame Columns: [] Index: []*)

Шлях за замовчуванням для розділення даних навчання, перевірки, тестування.

**Parameters**:

- **data_with_features** (`DataFrame`) – Вхідні дані
- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **test_fraction** (`float`) – частина даних для тестування
- **backtest** (`bool`) – логічна величина, якщо нам потрібно зробити бектест
- **test_data_predefined**(`DataFrame`) – Попередньо визначений фрейм даних тесту, який буде використовуватися в трубопроводі (за замовчуванням порожній фрейм даних)

**Return type**:

`Tuple`[`DataFrame`, `DataFrame`, `DataFrame`, `DataFrame`]

**Returns**:

- Train dataset (навчальний набір даних)
- Validation dataset (превірочний набір даних)
- Test dataset (тестувальний набір даних)

openstef.pipeline.train_model.train_pipeline_step_train_model(pj, model_specs, train_data, validation_data)

Навчання моделі.

**Parameters**:
- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **model_specs**(`ModelSpecificationDataClass`) – Клас даних, що містить характеристики моделі
- **train_data** (`DataFrame`) – навчальні дані
- **validation_data** (`DataFrame`) – тестувальні дані

**Return type**:

[`OpenstfRegressor`](https://openstef.github.io/openstef/openstef.model.regressors.html#openstef.model.regressors.regressor.OpenstfRegressor)

**Returns**:

Навчана модель

**Raises**:

- **NotImplementedError** – У разі використання невірного типу моделі в роботі прогнозування.
- **InputDataWrongColumnOrderError** – Коли стовпець "load" не є першим, а стовпець "horizon" не є останнім.

## openstef.pipeline.utils module

**openstef.pipeline.utils.generate_forecast_datetime_range**(*forecast_data*)

Генерує прогнозний діапазон на основі останнього кластера нульових значень у першому цільовому стовпчику прогнозних даних.

### Приклад

На вхід цієї функції подається набір прогнозних даних з даними між 2021-11-05 та 2021-11-19, а також цільовий стовпець 'load' як перший стовпець. Перший стовпець 'load' має нульові значення між 2021-11-17 04:00:00 та 2021-11-19 05:00:00. Нульові значення в кінці стовпчика вказують на час, коли потрібні прогнози. Тому ця функція встановлює час початку прогнозів як 2021-11-17 04:00:00 і час закінчення прогнозів як 2021-11-19 05:00:00.

**Parameters**:

forecast_data (`DataFrame`) – Датафрейм прогнозу.

**Return type**:

`tuple`[`datetime`, `datetime`]

**Returns**:

Початок і кінець діапазону прогнозування.

**Raises**:

**ValueError** – Якщо цільовий стовпець не має нульових значень.