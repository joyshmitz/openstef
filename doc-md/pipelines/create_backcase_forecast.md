# openstef.pipeline.create_basecase_forecast module

**openstef.pipeline.create_basecase_forecast.create_basecase_forecast_pipeline**(*pj, input_data*)

Обчислює прогноз базового сценарію та довірчі інтервали для заданої роботи прогнозування і вхідних даних.

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота прогнозування
- **input_data** (`DataFrame`) – фрейм даних, що містить вхідні дані, необхідні для прогнозування.

**Return type**:

`DataFrame`

**Returns**:

Прогноз для базового сценарію

**Raises**:

**NoRealisedLoadError** – Коли для заданого діапазону даних відсутнє реальне навантаження.

**openstef.pipeline.create_basecase_forecast.generate_basecase_confidence_interval**(*data_with_features*)

Розрахувує довірчий інтервал для базового прогнозу.

**Parameters**:

**data_with_features** (`DataFrame`) – Вхідні дані, які використовуються для побудови базового прогнозу.

**Return type**:

`DataFrame`

**Returns**:

Фрейм даних з довірчим інтервалом.