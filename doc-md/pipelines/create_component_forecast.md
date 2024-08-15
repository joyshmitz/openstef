# openstef.pipeline.create_component_forecast module

**openstef.pipeline.create_component_forecast.create_components_forecast_pipeline**(*pj, input_data, weather_data*)

Трубопровід для створення составного прогнозу за допомогою моделі прогнозування Dazls.

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота передбачення
- **input_data** (`DataFrame`) – Прогноз вхідних даних для составного прогнозу.
- **weather_data** (`DataFrame`) – Метеодані зі стовпчиками 'радіація' та 'швидкість вітру_100м'

**Return type**:

`DataFrame`

**Returns**:

Фрейм даних зі составним прогнозом. Фрейм даних містить такі стовпці: “forecast_wind_on_shore”, “forecast_solar”, “forecast_other”, “pid”, “customer”, “description”, “type”, “algtype”

**openstef.pipeline.create_component_forecast.create_input**(*pj, input_data, weather_data*)

Ця функція готує вхідні дані.

Ці дані будуть використані для прогнозування за моделлю Dazls, тому вони будуть відповідати вимогам моделі Dazls.

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота передбачення
- **input_data** (`DataFrame`) – Прогноз вхідних даних для составного прогнозу.
- **weather_data** (`DataFrame`) – Метеодані зі стовпчиками 'радіація' та 'швидкість вітру_100м'

**Return type**:

`DataFrame`

**Returns**:

Він виводить фрейм даних, який буде використано для функції прогнозування Dazls.
