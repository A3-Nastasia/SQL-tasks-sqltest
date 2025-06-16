## Задание 1
Выберите все записи из таблицы `actor`.
```
SELECT * FROM actor
```

## Задание 2
Напишите SQL-запрос для выбора столбцов `sex` - пол и `body_mass_g` - масса тела из таблицы `little_penguins`, отсортированных таким образом, чтобы сначала отображалась пингвины с наибольшей масса тела.
```
SELECT sex, body_mass_g
FROM little_penguins

ORDER BY body_mass_g DESC
```

## Задание 3
Получите все записи из таблицы `address`, для которых не указан почтовый индекс.
```
SELECT * FROM address
WHERE postal_code IS NULL
```

## Задание 4
Получите столбец name из таблицы language в алфавитном порядке.
```
SELECT name
FROM language

ORDER BY name
```

## Задание 5
Выберите все значения имен и фамилий актеров из таблицы `actor`.
```
SELECT first_name, last_name
FROM actor
```

## Задание 6
Получите список значений из колонки `name` таблицы `language`.
```
SELECT name
FROM language
```

## Задание 7
Выберите названия фильмов из таблицы `film`. \
Отсортируйте полученный список по алфавиту
```
SELECT title
FROM film

ORDER BY title ASC
```

## Задание 8
Из таблицы customer выберите все записи о фамилии - `last_name`, имени - `first_name` и адресе электронной почты `email` отсортировав их по фамилии в алфавитном порядке.
```
SELECT last_name, first_name, email
FROM customer

ORDER BY last_name ASC
```

## Задание 9
Напишите SQL запрос, который выводит список уникальных значений `rating` из таблицы `film` в алфавитном порядке.
```
SELECT DISTINCT rating
FROM film

ORDER BY rating
```

## Задание 10
Получите названия пяти самых длинных фильмов, отсортированных по продолжительности в порядке убывания.
```
SELECT title
FROM film

ORDER BY length DESC
LIMIT 5
```

## Задание 11
Выберите название, описание и год выхода фильмов из таблицы `film`. \
Отсортируйте полученный список по названию в алфавитном порядке и выведите первые десять строк
```
SELECT title, description, release_year
FROM film

ORDER BY title ASC
LIMIT 10
```

## Задание 12
Для удобства показа мы разобьем список фильмов на страницы по десять записей на каждой. \
Для формирования третьей страницы списка выберите название, описание и год выхода фильмов из таблицы `film`. \
Отсортируйте полученный список по названию в алфавитном порядке и выведите десять строк начиная с двадцать первой.
```
SELECT title, description, release_year
FROM film

ORDER BY title ASC
LIMIT 10
OFFSET 20
```

## Задание 13
Выберите название, стоимость проката и продолжительность фильмов из таблицы `film`. \
Отсортируйте полученный список по убыванию стоимости, фильмы с одинаковой стоимостью отсортируйте по возрастанию продолжительности фильма.
```
SELECT title, rental_rate, length
FROM film
ORDER BY rental_rate DESC, length ASC
```

## Задание 14
Найдите самый длинный фильм в таблице `film`. \
Если несколько фильмов имеют одинаковую продолжительность, выберите фильм с наименьшей ценой замены `replacement_cost`. \
Напишите запрос, без использования агрегатых функций, который возвращает два столбца: `title` и `release_year`.
```
SELECT title, release_year
FROM film

ORDER BY length DESC, replacement_cost ASC
LIMIT 1
```

## Задание 15
Найдите все фильмы продолжительностью более трёх часов. \
Напишите SQL запрос возвращающий результат состоящий из трёх столбцов: названия фильма, его описания и продолжительности в минутах отсортированный по длине фильма.
```
SELECT title, description, length
FROM film

WHERE length>180
ORDER BY length
```

## Задание 16
Найдите сотрудников, работающих в магазине номер 1, и получите все их данные.
```
SELECT *
FROM staff

WHERE store_id = 1
```

## Задание 17
Найдите всех активных в данный момент клиентов (`active` = 1) в таблице `customer`. \
Таблица результатов должна содержать следующие поля: `customer_id`, `first_name` и `last_name`.
```
SELECT customer_id, first_name, last_name
FROM customer

WHERE active = 1
```

## Задание 18
Найдите актеров по имени **Scarlett**.
```
SELECT *
FROM actor

WHERE first_name = 'Scarlett'
```

## Задание 19
Найдите все фильмы, в описании которых есть слово **Student**. Выведите названия фильмов в алфавитном порядке.
```
SELECT title
FROM film

WHERE description LIKE '%Student%'
ORDER BY title ASC
```

## Задание 20
Найдите все фильмы продолжительностью более 3 часов и получите их название, год выпуска и продолжительность, отсортированные по продолжительности в порядке возрастания.
```
SELECT title, release_year, length
FROM film

WHERE length > 180
ORDER BY length ASC
```

## Задание 21
Найдите все комедии продолжительностью более трёх часов. \
Напишите SQL запрос возвращающий результат состоящий из трёх столбцов: названия фильма, года выхода на экран и продолжительности в минутах отсортированный по длине фильма.
```
SELECT f.title, f.release_year, f.length
FROM film f

INNER JOIN film_category fc
ON f.film_id = fc.film_id

INNER JOIN category c
ON fc.category_id = c.category_id

WHERE c.name = 'Comedy' AND length > 180
ORDER BY f.length
```

## Задание 22
Выберите фамилии, имена и адреса электронной почты клиентов, чьи имя и фамилия не содержат ни одной буквы «А» (латинская буква). \
Отсортируйте результат по `customer_id`
```
SELECT last_name, first_name, email
FROM customer

WHERE first_name NOT LIKE '%A%' AND last_name NOT LIKE '%A%'
ORDER BY customer_id
```

## Задание 23
Найдите все фильмы с рейтингом **NC-17** (только для взрослых), в описании которых содержится подстрока **Database Administrator**. \
Выведите название, описание, год выпуска этих фильмов в алфавитном порядке по названию.
```
SELECT title, description, release_year
FROM film

WHERE rating = 'NC-17' AND description LIKE '%Database Administrator%'
ORDER BY title ASC
```

## Задание 24
Найдите все фильмы, в описании которых есть слова **Dog** или **Cat**, отмеченные рейтингом **PG** или **PG-13** (для просмотра под контролем родителей). \
Выведите названия, описания, годы выпуска этих фильмов, отсортировав по названию в алфавитном порядке.
```
SELECT title, description, release_year
FROM film

WHERE (description LIKE '%Dog%' OR description LIKE '%Cat%') AND
      (rating = 'PG' OR rating = 'PG-13')
ORDER BY title ASC
```

## Задание 25
Фильмы с рейтингом **R** (Ограниченный доступ) и **NC-17** (Только для взрослых) не могут быть взяты напрокат молодежью. \
Получите список этих фильмов в две колонки `title` и `rating`, отсортированных по названию фильма. \
Для решения этой задачи используйте условие с ключевым словом `OR`.
```
SELECT title, rating 
FROM film

WHERE rating = 'R' OR rating = 'NC-17'
ORDER BY title ASC
```

## Задание 26
Фильмы с рейтингом **PG** (рекомендуется родительский контроль) и **PG-13** (родители должны быть осторожны) могут просматриваться детьми только под контролем родителей.
Получите список этих фильмов в двух столбцах `title`, `rating`, отсортированных по названию.
```
SELECT title, rating
FROM film

WHERE rating = 'PG' OR rating = 'PG-13'
ORDER BY title ASC
```

## Задание 27
Найдите всех сотрудников, занятых на проекте "**Video Database**". \
Напишите запрос, который выводит номер сотрудника, имя, фамилию, дату приёма на работу и код должности. \
Отсортируйте результат по фамилиям в алфавитном порядке. Если фамилии совпадают, отсортируйте по коду должности.
```
SELECT e.EMP_NO, e.FIRST_NAME, e.LAST_NAME, e.HIRE_DATE, e.JOB_CODE
FROM EMPLOYEE e

INNER JOIN EMPLOYEE_PROJECT ep
ON e.EMP_NO = ep.EMP_NO

INNER JOIN PROJECT p
ON ep.PROJ_ID = p.PROJ_ID

WHERE p.PROJ_NAME = 'Video Database'
ORDER BY e.LAST_NAME ASC, e.JOB_CODE ASC
```

## Задание 28
Напишите запрос, извлекающий список всех сотрудников, работающих за пределами **США**. \
Результат должен содержать все столбцы таблицы `EMPLOYEE`.
```
SELECT *
FROM EMPLOYEE

WHERE JOB_COUNTRY != 'USA'
```

## Задание 29 (Прикол с Firebird)
Напишите запрос, извлекающий список всех сотрудников, принятых на работу в **1992** году.
Результат должен содержать следующие столбцы `FULL_NAME` - полное имя сотрудника и `HIRE_DATE` - дата приёма на работу. Отсортируйте результат по возрастанию даты приёма
```
SELECT FULL_NAME, HIRE_DATE
FROM EMPLOYEE

WHERE EXTRACT(YEAR FROM HIRE_DATE) = 1992
ORDER BY HIRE_DATE ASC
```

## Задание 30
Напишите SQL запрос, чтобы получить список фильмов, отсутствующих в прокате (таблица `inventory`). \
Отобразите названия этих фильмов в столбце с названием `film_title` в алфавитном порядке. \
Используйте для решения задачи соединение таблиц.
```
SELECT f.title AS film_title
FROM film f

LEFT JOIN inventory i
ON i.film_id = f.film_id

WHERE i.film_id IS NULL
ORDER BY film_title ASC
```

## Задание 31
Напишите SQL запрос для получения списка языков из таблицы `language`, на которых нет доступных фильмов. \
Представьте результат в таблице с одним столбцом - `language`, отсортированным по алфавиту. \
Используйте для решения задачи соединение таблиц.
```
SELECT DISTINCT name AS language
FROM language l

LEFT JOIN film f
ON l.language_id = f.language_id

WHERE f.language_id IS NULL
ORDER BY language
```

## Задание 32
Напишите SQL запрос, который выводит названия всех фильмов и их категорий из базы данных Sakila.
```
SELECT f.title, c.name
FROM film_category fc

INNER JOIN film f
ON fc.film_id = f.film_id

INNER JOIN category c
ON fc.category_id = c.category_id
```

## Задание 33
Извлеките имя и домен из адресов электронной почты клиентов в базе данных Sakila. \
Напишите запрос, возвращающий три столбца: `email`, `address` – часть адреса электронной почты перед знаком «**@**» и `domain` — часть после «**@**». \
Отсортируйте результат по полю `email`.
```
SELECT email, 
       SUBSTRING_INDEX(email, '@', 1) AS address,
       SUBSTRING_INDEX(email, '@', -1) AS address
FROM customer

ORDER BY email
```

## Задание 34
Получить определения столбцов таблицы `address`.
```
SHOW COLUMNS FROM address
```

## Задание 35
Получить список индексов таблицы `film` и их определений.
```
SHOW INDEX FROM film
```

## Задание 36
Найдите фильмы из базы данных Sakila, для которых нет записей об учавствоваших в них актёрах используя соединение таблиц `JOIN`. \
Выведите результирующую с полями `title`, `release_year` отсортированных по названию фильма.
```
SELECT f.title, f.release_year
FROM film f

LEFT JOIN film_actor fa
ON f.film_id = fa.film_id

WHERE fa.actor_id IS NULL
ORDER BY f.title
```

## Задание 37
Найдите клиентов чьё имя является фамилией другого клиента. Выведите таблицу с полями `customer_id`, `first_name`, `last_name` для первого клиента и такие же поля `customer_id`, `first_name`, `last_name` для второго. Отсортируйте по `customer_id` первого клиента.
```
SELECT c1.customer_id, c1.first_name, c1.last_name, c2.customer_id, c2.first_name, c2.last_name
FROM customer c1

INNER JOIN customer c2
ON c1.first_name = c2.last_name

ORDER BY c1.customer_id
```

## Задание 38
Найдите клиентов которые встречали друг друга в одном из пунктов проката. Выведите таблицу с полями `meet_time` - согласно времени аренды, `store_id` - место встречи, `customers` список встречавшихся клиентов в формате **JOHN SHOW**,**DAENERYS TARGARYEN** - в порядке их фамилий. \
Результирующую таблицу отсортируйте по времени встречи и номеру пункта проката \
(Клиенты встречались если брали в аренду фильмы в одном отделении в одно время. Место встречи определяется местом работы сотрудника.)
```
SELECT r.rental_date AS meet_time,
       s.store_id,
       GROUP_CONCAT(DISTINCT CONCAT(c.first_name, ' ', c.last_name) ORDER BY c.last_name SEPARATOR ',') AS customers
FROM rental r

INNER JOIN customer c
ON r.customer_id = c.customer_id

INNER JOIN staff s
ON r.staff_id = s.staff_id

GROUP BY meet_time, s.store_id
HAVING COUNT(DISTINCT r.customer_id) > 1
ORDER BY meet_time, s.store_id
```

## Задание 39
Напишите SQL запрос для поиска фильмов в базе данных Sakila, которые есть в наличии (в таблице `inventory`), но никогда не выдавались в прокат. \
Выведите названия этих фильмов в алфавитном порядке. \
Для решения задачи используйте соединение таблиц.
```
SELECT title
FROM inventory i

LEFT JOIN rental r
ON i.inventory_id = r.inventory_id

INNER JOIN film f
ON f.film_id = i.film_id

WHERE r.inventory_id IS NULL
ORDER BY title
```

## Задание 40
Получите все фильмы в следующих категориях: **Comedy**, **Music** и **Travel**. Выведите таблицу со столбцами `film_id`, `title` и `category`, отсортированными по `film_id`. Напишите запрос без использования ключевого слова `OR` в условии.
```
SELECT f.film_id, f.title, c.name AS category
FROM film f

INNER JOIN film_category fc
ON f.film_id = fc.film_id

INNER JOIN category c
ON fc.category_id = c.category_id

WHERE c.name IN ('Comedy', 'Music', 'Travel')
ORDER BY film_id
```

## Задание 41
Выберите имена и фамилии клиентов, чьи имя и фамилия начинаются на одну и ту же букву. \
Отсортируйте результат по имени и фамилии.
```
SELECT first_name, last_name
FROM customer

WHERE LEFT(first_name, 1)=LEFT(last_name, 1)
ORDER BY first_name ASC, last_name ASC
```

## Задание 42
Найдите все фильмы взятые в прокат **KATIE ELLIOTT**. Выведите результат в два столбца `title` и `rating`. \
Отсортируйте список так что бы сначала шли фильмы "для взрослых" (с рейтингом **R**), а затем все остальные по алфавиту.
```
SELECT f.title, f.rating
FROM rental r

INNER JOIN customer c
ON r.customer_id = c.customer_id

INNER JOIN inventory i
ON r.inventory_id = i.inventory_id

INNER JOIN film f
ON i.film_id = f.film_id

WHERE c.first_name = 'KATIE' AND c.last_name = 'ELLIOTT'
ORDER BY (f.rating = 'R') DESC, title ASC
```