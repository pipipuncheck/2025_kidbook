# Отчет по лабораторной работе

## Состав команды

| ФИО                          | Что делал                           | Оценка |
|------------------------------|-------------------------------------|--------|
| Мунтяну Алексей Владимирович | Концептуализация предметной области |      |
| Днепров Степан Сергеевич     | Автоматизация написания текстов     | |
| Новиков Никита Сергеевич     | Автоматизация расставления ссылок   |  |

## Концептуализация предметной области

### 1) Анализ предметной области

- Определили ключевые понятия из раздела "Искусство и творчество".

- Использовали WikiData и DBpedia для извлечения структурированных данных.

### 2) Построение онтологии

 - Выделили 15 понятий с иерархическими связями (subclass) и горизонтальными (inspires, related to, uses и т.д.).

 - Визуализировали онтологию в виде [графа](Ontology.png).

### 3) Инструменты

- SPARQL-запросы (извлекали данные из WikiData).

- VisualParadigm Online (для визуализации связей).


## Написание текстов

 ### 1) Генерация контента
- Использование LLM API

- Написали [скрипт](generate_texts.py), который через API GigaChat генерировал markdown-файлы и [concepts](concepts.json), принимая на вход файл [articles](articles.json).

- Промпт: "Напиши статью о понятии [понятии], за основу возьми данный текст на английском: [текст с википедии] перескажи его на русском так, чтобы понял десятилетний ребенок, но определение должно быть строгим, без сюсюканья, в ответе не пиши ничего лишнего, никаких приветствий, вопросов "Что такое" и прочего, в статье не должно быть заголовков, по возможности используй в статье какие-то из этих понятий: [список всех понятий]".
- 

### 2) Ручная доработка

- Проверили тексты на мусорные пункты и заголовки.

## Расстановка ссылок

### 1) Автоматизация

- [Скрипт](add_links.py) на Python искал термины из [concepts](concepts.json), с помощью регулярного выражения разбивал текст на отдельные слова, с учетом пунктуации и т.д., далее каждое слово pymorphy3 перевел каждое слово в начальную форму и проверил входит ли начальная форма в список терминов.

## Выводы

### Сложности и решения

#### 1) Проблемы со SPARQL

- Некоторые запросы возвращали неполные данные → вручную дополнили онтологию.

#### 2) Ограничения LLM

- Иногда модель выдавала тексты, имеющие слишком мало смысла → добавили уточняющий промпт: "определение должно быть строгим, без сюсюканья".
- Иногда модель писала лишние, мусорные заголовки (Что такое [понятие]) → добавили уточняющий промпт: "в ответе не пиши ничего лишнего, никаких приветствий, вопросов "Что такое" и прочего, в статье не должно быть заголовков".
- Тексты получались проктически без перекрестных ссылок, причем в статьях встречались синонимы терминов → добавили уточняющий промпт: "по возможности используй в статье какие-то из этих понятий: [список всех понятий]".

### Что получилось?
✅ Полноценная онтология с 15 понятиями.

✅ Автоматическая генерация и линковка текстов.

✅ Удобные markdown-файлы c простым объяснением всех понятий.

### Что можно улучшить?
🔹 Добавить перекрестные ссылки на понятия из других групп.

🔹 Написать программу, которая будет использовать GigaChat API, запрашивая сначала список терминов, относящихся к предметной области, затем генерируя SPARQL запрос, и на его основе генерировать файл articles.json