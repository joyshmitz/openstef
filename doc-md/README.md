<!--
SPDX-FileCopyrightText: 2017-2023 Contributors to the OpenSTEF project <korte.termijn.prognoses@alliander.com>

SPDX-License-Identifier: MPL-2.0
-->

# Інструкції до документації

Документація створюється за допомогою Sphinx:
https://www.sphinx-doc.org/en/master/index.html

Для кожного *pull_request*:
- перевірити, чи можна згенерувати документацію

Для кожого *release*:
1. Дія на github запускає створення документації
    1. sphinx-apidoc автоматично генерує .rst файли на основі вихідного коду
    2. sphinx-build автоматично генерує .html файли з rst-файлів
2. Дія github публікує результати у гілці `gh_pages`.
3. github автоматично перетворює `gh_pages` на веб-сайт

Важливі файли:
- `conf.py`: визначення налаштувань
- `Makefile` і `make.bat`: не зовсім впевнений, ми використовуємо значення за замовчуванням
- `index.rst`: визначає індекс фінальної документації

Під час локального запуску, html-файли документації генеруються, але не додаються до gh-сторінок:
```
pip install -r requirements.txt
pip install -r docs/doc-requirements.txt
sphinx-apidoc -o docs openstef
sphinx-build docs output
```

Запустіть форматування та перевірку рядка документа локально:
```
pip install -r requirements.txt
pip install -r docs/doc-requirements.txt
pydocstyle .
docformatter openstef --recursive --wrap-summaries 120 --in-place
```
