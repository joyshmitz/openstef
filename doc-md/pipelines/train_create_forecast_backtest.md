# openstef.pipeline.train_create_forecast_backtest module

**openstef.pipeline.train_create_forecast_backtest.train_model_and_forecast_back_test**(*pj, modelspecs, input_data, training_horizons=None, n_folds=1*)

Трубопровід для зворотного тесту.

Якщо кількість згинів більша за 1: застосувати трубопровод для зворотного тесту при прогнозуванні всього вхідного діапазону.

> - Використовує k-кратну перехресну перевірку для того, щоб розділити дані кілька разів.
> - Результати всіх тестових наборів додаються разом для отримання прогнозу для всього вхідного діапазону.
> - Отримання днів для кожного згину може бути як випадковим, так і не випадковим.
>
>НЕ ВИКОРИСТОВУЙТЕ ЦЕЙ ТРУБОПРОВІД ДЛЯ ОПЕРАТИВНИХ ПРОГНОЗІВ

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота передбачення.
- **modelspecs** (`ModelSpecificationDataClass`) – Клас даних, що містить характеристики моделі
- **input_data** (`DataFrame`) – Input data
- **training_horizons** (`Optional`[`list`[`float`]]) – горизонти для тренувань у годинах. Ці горизонти також використовуються для прогнозування (по одному для кожного горизонту)
- **n_folds** (`int`) – кількість згинів для застосування (якщо 1, перехресна перевірка не застосовується)

**Return type**:

`tuple` [`DataFrame`, `list`[`OpenstfRegressor`], `list`[`DataFrame`], `list`[`DataFrame`], `list`[`DataFrame`]]

**Returns**:

- Forecast (pandas.DataFrame)

- Fitted models (list[OpenStfRegressor])

- Train data sets (list[pd.DataFrame])

- Validation data sets (list[pd.DataFrame])

- Test data sets (list[pd.DataFrame])

**Raises**:

- **InputDataInsufficientError** – коли вхідних даних недостатньо.
- **InputDataWrongColumnOrderError** – коли вхідні дані мають неправильний порядок стовпців.
- **ValueError** – якщо горизонт являє собою рядок, а відповідного стовпчика немає у вхідних даних
- **InputDataOngoingZeroFlatlinerError** – коли всі останні вимірювання навантаження дорівнюють нулю.

**openstef.pipeline.train_create_forecast_backtest.train_model_and_forecast_test_core**(*pj, modelspecs, train_data, validation_data, test_data*)

Тренує модель і прогнозує на тестовому наборі.

**Parameters**:

- **pj** (`PredictionJobDataClass`) – Робота передбачення.
- **modelspecs** (`ModelSpecificationDataClass`) – Клас даних, що містить характеристики моделі
- **train_data** (`DataFrame`) – Навчальні дані з обчислюваними функціями
- **validation_data** (`DataFrame`) – Дані перевірки з обчислювальними функціями
- **test_data** (`DataFrame`) – Тестові дані з обчисленими функціями

**Return type**:

`tuple`[`OpenstfRegressor`, `DataFrame`]

**Returns**:

- Навчена модель
- Прогноз на тестовій вибірці.

**Raises**:

- **NotImplementedError** – У разі використання невірного типу моделі в роботі прогнозування.
- **InputDataWrongColumnOrderError** – Коли стовпець 'load' не є першим, а стовпець 'horizon' не є останнім.
