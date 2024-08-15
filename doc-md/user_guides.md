# Керівництво користувача

TЦя сторінка містить інструкції та посилання на ресурси, які показують,
як можна використовувати OpenSTEF.

## Трубопроводи - функціональність високого рівня

OpenSTEF розроблено на основі трубопроводів (див. `concepts <concepts>`
для визначення). Трубопроводи пропонують простий спосіб навчання
моделей, генерування прогнозів та оцінювання ефективності прогнозування.

Доступні наступні трубопроводи:

-   `openstef.pipeline.train_model`
-   `openstef.pipeline.create_forecast`
-   `openstef.pipeline.optimize_hyperparameters`
-   `openstef.pipeline.create_component_forecast`
-   `openstef.pipeline.create_basecase_forecast`
-   `openstef.pipeline.train_create_forecast_backtest`

Чудовий спосіб розпочати роботу та ознайомитися з трубопроводами
OpenSTEF - це поглянути на [це репозиторій GitHub, який містить
набір прикладів у блокноті Jupyter](https://github.com/OpenSTEF/openstef-offline-example).
Репозиторій навіть містить приклади даних.

Ви можете запустити кожен приклад блокнота локально без будь-яких
налаштувань, окрім [installation of the OpenSTEF
package](https://pypi.org/project/openstef/).

Ми рекомендуємо вам ознайомитися з усіма прикладами, але ось список, з
якого ви можете почати:

-   [Як тренувати модель](https://github.com/OpenSTEF/openstef-offline-example/blob/master/examples/01.%20Train%20a%20model%20using%20high-level%20pipelines.ipynb).
-   [Як створити прогноз](https://github.com/OpenSTEF/openstef-offline-example/blob/master/examples/04.%20Test_on_difficult_cases.ipynb).
-   [Як оцінити продуктивність моделі за допомогою бек-тесту](https://github.com/OpenSTEF/openstef-offline-example/blob/master/examples/02.%20Evaluate%20performance%20using%20Backtest%20Pipeline.ipynb).

Більш детальну інформацію про те, як використовувати та реалізовувати
трубопроводи у робочому середовищі, включно з прикладами коду, можна
знайти у розділі `pipeline_user_guide` цієї документації.

## Розгортання як повноцінного додатку для прогнозування

Якщо ви хочете налаштувати повноцінну програму прогнозування, готову до
використання в робочому середовищі з внутрішнім сховищем даних і
графічним інтерфейсом користувача, це [Репозиторій GitHub, що містить
референсну реалізацію](https://github.com/OpenSTEF/openstef-reference) ви
можете використовувати як відправну точку. Цей приклад реалізації
включає бази даних, користувацький інтерфейс та приклади даних.

Більше інформації про те, як може виглядати архітектура такого додатку,
можна знайти `тут <application-architecture>`.

Скріншот операційної панелі, що показує ключові функціональні можливості
OpenSTEF. Документацію до інформаційної панелі можна знайти
[тут](https://raw.githack.com/OpenSTEF/.github/main/profile/html/openstef_dashboard_doc.html).

## Приклади блокнотів Jupyter

Блокноти Jupyter, що демонструють деякі з основних функціональних
можливостей OpenSTEF, можна знайти за адресою:
<https://github.com/OpenSTEF/openstef-offline-example>.
