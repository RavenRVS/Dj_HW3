# Часть 1. Выгрузка каталога товаров из csv-файла с сохранением всех позиций в базе данных

## Задание <a href="https://github.com/RavenRVS/DJ_HW3_T1/blob/master/README.md#doc">(Мое решение)</a> 

Есть некоторый [csv-файл](./phones.csv), который выгружается с сайта-партнера. Этот сайт занимается продажей телефонов.

Мы же являемся их региональными представителями, поэтому нам необходимо взять данные из этого файла и отобразить их на нашем сайте на странице каталога, с их предварительным сохранением в базу данных.

## Реализация

Что необходимо сделать:

- В файле `models.py` нашего приложения создаем модель Phone с полями `id`, `name`, `price`, `image`, `release_date`, `lte_exists` и `slug`. Поле `id` - должно быть основным ключом модели.
- Значение поля `slug` должно устанавливаться слагифицированным значением поля `name`.
- Написать скрипт для переноса данных из csv-файла в модель `Phone`.
  Скрипт необходимо разместить в файле `import_phones.py` в методе `handle(self, *args, **options)`.
  Подробнее про подобные скрипты (django command) можно почитать [здесь](https://docs.djangoproject.com/en/3.2/howto/custom-management-commands/) и [здесь](https://habr.com/ru/post/415049/).
- При запросе `<имя_сайта>/catalog` - должна открываться страница с отображением всех телефонов.
- При запросе `<имя_сайта>/catalog/iphone-x` - должна открываться страница с отображением информации по телефону. `iphone-x` - это для примера, данное значние берется из `slug`.
- В каталоге необходимо добавить возможность менять порядок отображения товаров: по названию (в алфавитном порядке) и по цене (по-убыванию и по-возрастанию).


## Ожидаемый результат

![Каталог с телефонами](https://github.com/RavenRVS/DJ_HW3_T1/blob/master/res/catalog.png)



# Часть 2. Делаем онлайн-библиотеку

## Задание <a href="https://github.com/RavenRVS/DJ_HW3_T2/blob/master/README.md#doc">(Мое решение)</a> 

Необходимо сделать онлайн библиотеку с каталогом книг. Библиотека должна состоять из двух страниц:

- `/books/` - отображение списка книг
- `/books/2021-01-02/` - отображение списка книг за дату 2021-01-02 (год, месяц, день)

Книга имеет три параметра:

- Название
- Автор
- Дата публикации (pub_date)

Так же на странице `/books/<pub_date>/` сделать возможность пагинации на страницу с книгами предыдущей даты и следующей даты.

Например: в библиотеке имеется 4 книги: одна за пятое число условного месяца, вторая за третье число того же месяца, третья за десятое и последняя за одиннадцатое. На странице `/books/` отображаем все эти книги. А на странице `/books/2021-05-05/` отображаем первую книгу, и ссылки на страницу с книгами за предыдущую дату (2021-05-03) и следующую дату (2021-05-10).

## Ожидаемый результат

![Каталог со всеми книгами](https://github.com/RavenRVS/DJ_HW3_T2/blob/master/res/catalog_1.png)

![Каталог с книгами выбранной даты публикования](https://github.com/RavenRVS/DJ_HW3_T2/blob/master/res/catalog_2.png)


# Часть 3. Миграции

## Задание <a href="https://github.com/RavenRVS/DJ_HW4_T1/blob/master/README.md#doc">(Мое решение)</a> 

Есть страница сайта школы.
При создании был не верно выбран тип связи `многие к одному`.
И теперь у ученика есть только один учитель и чтобы добавить к нему второго, требуется
заново добавлять ученика в базу.

Необходимо поменять модели и сделать отношение `многие ко многим` между Учителями и Учащимися.
Это решит проблемы текущей архитектуры.

Задача:

1. Поменять отношения моделей `Student` и `Teacher` с `Foreign key` на `Many to many`
2. Поправить шаблон списка учеников с учетом изменения моделей

## Примечание

Сначала примените текущую миграцию, затем загрузите исходные данные с помощью `loaddata` и после этого меняйте модели и делайте новые миграции.

В противном случае `loaddata` не сможет загрузить данные на новую схемы и данные придется заносить вручную.


## Дополнительное задание

Проанализируйте число sql-запросов (напоминание: для этого можно использовать `django-debug-toolbar`) Для каждого студента будет выполняться отдельный запрос. Это не очень производительное решение - улучшите его с помощью `prefetch_related` ([документация](https://docs.djangoproject.com/en/3.2/ref/models/querysets/#prefetch-related)).


# Часть 4. Связь многие-ко-многим

## Задание <a href="https://github.com/RavenRVS/DJ_HW4_T2/blob/master/README.md#doc">(Мое решение)</a> 

Есть небольшой новостной сайт.

![Начальное состояние](https://github.com/RavenRVS/DJ_HW4_T2/blob/master/res/base.png)

Было решено к статьям добавить тематические резделы, к которым они относятся, и отображать их у каждой новости в виде списка тегов.

![Вывод тегов разделов](https://github.com/RavenRVS/DJ_HW4_T2/blob/master/res/with_tags.png)

У каждой статьи может быть несколько разделов, но всегда один из них должен быть основным.
В списке тегов он должен идти первым, потом все остальные в алфавитном порядке.

В админке необходимо реализовать создание разделов
и для страницы _Редактирование статьи_ добавить возможность указывать разделы.
Необходимо так же реализовать проверку на наличие одного и только одного основного раздела.

![Админка](https://github.com/RavenRVS/DJ_HW4_T2/blob/master/res/admin.gif)

## Примечание

* `Tag` - просто тег, только его название, ничего более.
* `Article` - статья с текстом, заголовком, картинкой и пр. + набор тегов (многие ко многим)
* `Scope` - таблица связка между статьей и тегом. Именно здесь должно быть свойство `is_main`

---

Вам нельзя менять шаблон! Ваша задача реализовать модели и логику так, чтобы текущий шаблон заработал (используйте `related_name`). 
