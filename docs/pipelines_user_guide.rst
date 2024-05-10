.. comment:
    SPDX-FileCopyrightText: 2017-2023 Contributors to the OpenSTEF project <korte.termijn.prognoses@alliander.com>
    SPDX-License-Identifier: MPL-2.0

.. _pipeline_user_guide:

Посібник користувача з трубопроводів
====================================

Як згадувалося у розділі :ref:`concept <concepts>`, завдання є розширенням трубопроводів, які включають отримання даних з бази даних,
виклик винятків завдань і запис даних до бази даних. В операційному середовищі можна використовувати як завдання, так і трубопроводи.
Основна відмінність полягає в тому, що операційний додаток, який використовує функціональність завдань OpenSTEF, легше реалізувати,
в той час як функціональність трубопроводу пропонує більшу гнучкість з точки зору проектування та реалізації, а також більшу масштабованість.

Щоб проілюструвати завдання, а також трубопровід :ref:`concept <concepts>`, нижче наведено фрагменти коду для обох реалізацій.
Ці фрагменти коду показують два різні способи, якими функціональність трубопроводу OpenSTEF може бути інтегрована у додаток, що працює у робочому середовищі.

Реалізація завдань
-------------------

Спочатку розглянемо реалізацію задачі, як це зроблено у `GitHub репозиторії, що містить референсну реалізацію <https://github.com/OpenSTEF/openstef-reference>`_.
У випадку, якщо навчання моделі, налаштування гіперпараметрів або прогнозування передбачається запускати за певним розкладом, наприклад, за допомогою CronJobs,
реалізація завдання легко налаштовується.
Однак масштабованість цієї реалізації обмежена. Крім того, ця реалізація покладається на `з'єднувач баз даних OpenSTEF <https://pypi.org/project/openstef-dbc/>`_, ``openstef-dbc``,
що означає, що бази даних мають бути налаштовані відповідно до `референсної реалізації <https://github.com/OpenSTEF/openstef-reference>`_.
Нижче наведено фрагменти коду для різних типів задач, які демонструють використання функціональності задач OpenSTEF.

Зауважте, що за винятком імпорту, реалізація є однаковою для кожного типу задач. Об'єкт `config` є об'єктом `pydantic.BaseSettings`, що містить всі необхідні конфігурації, такі як імена користувачів, секрети, хости тощо.

Виконання задачі моделі тренування
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    import sys
    from pathlib import Path

    from openstef.tasks import train_model as task
    from openstef_dbc.database import DataBase
    from openstef_dbc.log import logging

    def main():
        # Initialize logging
        logging.configure_logging(loglevel=config.loglevel, runtime_env=config.env)
        # Initialize database connection
        database = DataBase(config)
        task.main(config=config, database=database)


    if __name__ == "__main__":
        main()


Створення виконання прогнозного завдання
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    import sys
    from pathlib import Path

    from openstef.tasks import create_forecast as task
    from openstef_dbc.database import DataBase
    from openstef_dbc.log import logging

    def main():
        # Initialize logging
        logging.configure_logging(loglevel=config.loglevel, runtime_env=config.env)
        # Initialize database connection
        database = DataBase(config)
        task.main(config=config, database=database)


    if __name__ == "__main__":
        main()


Оптимізація реалізації завдань з гіперпараметрами
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    from pathlib import Path

    from openstef.tasks import optimize_hyperparameters as task
    from openstef_dbc.database import DataBase
    from openstef_dbc.log import logging

    def main():
        # Initialize logging
        logging.configure_logging(loglevel=config.loglevel, runtime_env=config.env)
        # Initialize database connection
        database = DataBase(config)
        task.main(config=config, database=database)


    if __name__ == "__main__":
        main()


Створення компонентів для прогнозування виконання завдань
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    from pathlib import Path

    from openstef.tasks import create_components_forecast as task
    from openstef_dbc.database import DataBase
    from openstef_dbc.log import logging

    def main():
        # Initialize logging
        logging.configure_logging(loglevel=config.loglevel, runtime_env=config.env)
        # Initialize database connection
        database = DataBase(config)
        task.main(config=config, database=database)


    if __name__ == "__main__":
        main()


Створення базового сценарію реалізації завдання прогнозування
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
::

    from pathlib import Path

    from openstef.tasks import create_basecase_forecast as task
    from openstef_dbc.database import DataBase
    from openstef_dbc.log import logging

    def main():
        # Initialize logging
        logging.configure_logging(loglevel=config.loglevel, runtime_env=config.env)
        # Initialize database connection
        database = DataBase(config)
        task.main(config=config, database=database)


    if __name__ == "__main__":
        main()


Реалізація трубопроводу
-----------------------

Реалізація трубопроводу не покладається на `the OpenSTEF database connector <https://pypi.org/project/openstef-dbc/>`_, ``openstef-dbc``.
Тому трубопроводи можна використовувати разом з будь-яким типом налаштування бази даних, на відміну від завдань,
які вимагають, щоб бази даних були реалізовані відповідно до `reference implementation <https://github.com/OpenSTEF/openstef-reference>`_.

Більш масштабоване і, можливо, більш акуратне налаштування, ніж `реалізація за посиланням <https://github.com/OpenSTEF/openstef-reference>`_,
є надання функціональності трубопроводу OpenSTEF за допомогою API,
наприклад, за допомогою фреймворку `FastAPI <https://fastapi.tiangolo.com/>`_.
Наведений нижче фрагмент коду показує, як трубопроводи OpenSTEF можуть бути інтегровані у API за допомогою
`repository pattern <https://mpuig.github.io/Notes/fastapi_basics/02.repository_pattern/>`_::

    from typing import Any, List, Tuple

    import pandas as pd
    from openstef.data_classes.model_specifications import ModelSpecificationDataClass
    from openstef.data_classes.prediction_job import PredictionJobDataClass
    from openstef.metrics.reporter import Report
    from openstef.model.regressors.regressor import OpenstfRegressor
    from openstef.pipeline.create_basecase_forecast import create_basecase_forecast_pipeline
    from openstef.pipeline.create_forecast import create_forecast_pipeline_core
    from openstef.pipeline.optimize_hyperparameters import (
        optimize_hyperparameters_pipeline_core,
    )
    from openstef.pipeline.train_model import train_model_pipeline_core


    class OpenstefRepository:
        """Repository that exposes function to interact with OpenSTEF pipelines."""

        def forecast_pipeline(
            self,
            prediction_job: PredictionJobDataClass,
            input_data: pd.DataFrame,
            model: OpenstfRegressor,
            modelspecs: ModelSpecificationDataClass,
        ) -> pd.DataFrame:
            """Wrapper around the forecast pipeline of OpenSTEF.
            The input_data should contain a `load` column.
            """
            return create_forecast_pipeline_core(
                prediction_job, input_data, model, modelspecs
            )

        def basecase_forecast_pipeline(
            self,
            prediction_job: PredictionJobDataClass,
            input_data: pd.DataFrame,
        ) -> pd.DataFrame:
            """Wrapper around the basecase forecast pipeline of OpenSTEF.
            The input_data should contain a `load` column.
            """
            return create_basecase_forecast_pipeline(prediction_job, input_data)

        def train_pipeline(
            self,
            prediction_job: PredictionJobDataClass,
            modelspecs: ModelSpecificationDataClass,
            input_data: pd.DataFrame,
            horizons: List[float] = None,
            old_model: OpenstfRegressor = None,
        ) -> Tuple[
            OpenstfRegressor,
            Report,
            ModelSpecificationDataClass,
            Tuple[pd.DataFrame, pd.DataFrame, pd.DataFrame],
        ]:
            """Wrapper around the train model pipeline of OpenSTEF.
            The input_data should contain a `load` column.
            """
            return train_model_pipeline_core(
                prediction_job,
                modelspecs,
                input_data,
                old_model,
                horizons=horizons,
            )

        def optimize_hyperparameters_pipeline(
            self,
            prediction_job: PredictionJobDataClass,
            input_data: pd.DataFrame,
            n_trials: int,
            horizons: List[float] = None,
        ) -> Tuple[
            OpenstfRegressor, ModelSpecificationDataClass, Report, dict, int, dict[str, Any]
        ]:
            """Wrapper around the optimize hyperparameters pipeline of OpenSTEF.
            The input_data should contain a `load` column.
            """
            return optimize_hyperparameters_pipeline_core(
                prediction_job, input_data, horizons, n_trials
            )
