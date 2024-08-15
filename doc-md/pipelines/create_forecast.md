# openstef.pipeline.create_forecast module

**openstef.pipeline.create_forecast.create_forecast_pipeline**(*pj, input_data, mlflow_tracking_uri*)

Створює трубопровід прогнозів.

Це трубопровід верхнього рівня, який включає в себе завантаження останньої моделі для даної роботи прогнозування.

Очікувані ключі роботи прогнозування: “id”,

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **input_data** (`DataFrame`) – Вхідні дані для навчання (без функцій)
- **mlflow_tracking_uri** (`str`) – URI відстеження MlFlow

**Return type**:

`DataFrame`

**Returns**:

Датафрейм з прогнозуванням

**Raises**:

- **InputDataOngoingZeroFlatlinerError** – Коли всі останні вимірювання навантаження дорівнюють нулю.
- **LookupError** – Коли для заданої роботи прогнозування в MLflow не знайдено жодної моделі.

**openstef.pipeline.create_forecast.create_forecast_pipeline_core***(*pj, input_data, model, model_specs*)

Створює трубопровід прогнозу (кореневий).

Обчислює прогнози та довірчі інтервали на основі роботи прогнозування і вхідних даних. Цей трубопровід не має бази даних або постійних залежностей від сховища.

Очікувані ключі роботи прогнозування: “resolution_minutes”, “id”, “type”,
“name”, “quantiles”

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота прогнозування.
- **input_data** (`DataFrame`) – Вхідні дані для прогнозування.
- **model** (`OpenstfRegressor`) – Модель, що використовується для цього прогнозування.
- **model_specs** (`ModelSpecificationDataClass`) – Характеристики моделі.

**Return type**:

`DataFrame`

**Returns**:

Прогнозування

**Raises**:

**InputDataOngoingZeroFlatlinerError** – Коли всі останні вимірювання навантаження дорівнюють нулю.