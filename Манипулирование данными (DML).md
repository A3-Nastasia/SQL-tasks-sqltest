## Задание 1
Напишите запрос на добавление новой записи в таблицу `address` со следующими данными:
- адрес: **898 Homer St**
- округ: **Yaletown**
- код города: **565** (Ванкувер)
- почтовый индекс: **26336**
- телефон **0523323201**
```
INSERT INTO address (
    address, 
    district, 
    city_id, 
    postal_code, 
    phone
) 
VALUES (
    '898 Homer St',
    'Yaletown',
    565,
    26336,
    0523323201
)
```

## Задание 2
Обновите почтовый индекс в таблице address по адресу **1411 Lillydale Drive** на **365894**.
```
UPDATE address
SET
    postal_code = 365894
WHERE
    address = '1411 Lillydale Drive'
```

## Задание 3
Для всех адресов в **Woodridge** в таблице `address` установите значение `postal_code` **4114** там, где текущее значение равно **NULL**.
```
UPDATE address a

INNER JOIN city c
ON a.city_id = c.city_id

SET
    a.postal_code = 4114
WHERE
    c.city = 'Woodridge'
    AND a.postal_code IS NULL
```

## Задание 4
Обновите все почтовые индексы **Канады** в таблице `address`, добавив префикс **111** для каждого **ненулевого** почтового индекса.
```
UPDATE address a

INNER JOIN city c
ON a.city_id = c.city_id

INNER JOIN country co
ON c.country_id = co.country_id

SET
    a.postal_code =  CONCAT('111',a.postal_code)
WHERE
    co.country = 'Canada'
    AND postal_code IS NOT NULL
```

## Задание 5
Клиент **COLLEEN BURTON** был нанят компанией на работу в в филиале номер **2** Sakila. Введите его данные в таблицу `staff`, взяв имя, фамилию и адрес из таблицы `customer`. Составьте рабочий адрес электронной почты из имени и фамилии, разделенных точкой, и домена компании **sakilastaff.com**, используйте имя в нижнем регистре в качестве имени пользователя сотрудника, введите значение **8cb2237d0679ca88db6464eac60da96345513964** в поле пароля.
```
INSERT INTO staff (
    first_name,
    last_name,
    address_id,
    email,
    store_id,
    username,
    password
)

SELECT 
    c.first_name, 
    c.last_name, 
    c.address_id,
    CONCAT(c.first_name, '.', c.last_name, '@sakilastaff.com'),
    2,
    LOWER(c.first_name),
    '8cb2237d0679ca88db6464eac60da96345513964'
FROM 
    customer c
WHERE 
    c.first_name = 'COLLEEN' 
    AND c.last_name = 'BURTON'
```

## Задание 6
Создайте представление `customer_address` со следующими полями `customer_id`, `first_name`, `last_name`, `address`, `city` и `country`
```
CREATE VIEW customer_address AS
    SELECT cu.customer_id, 
           cu.first_name, 
           cu.last_name, 
           a.address, 
           c.city, 
           co.country
    FROM customer cu
    
    INNER JOIN address a
    ON cu.address_id = a.address_id
    
    INNER JOIN city c
    ON a.city_id = c.city_id
    
    INNER JOIN country co
    ON c.country_id = co.country_id
```

## Задание 7
На основе таблицы `penguins` создайте представление `penguins_islands_view` с полями `island` - название острова и `penguins_count` - количество проживающих на нём пингвинов.
```
CREATE VIEW penguins_islands_view AS
    SELECT island,
           COUNT(species) AS penguins_count
    FROM penguins
    GROUP BY island
```

## Задание 8
Удалите из таблицы `customer` записи о неактивных клиентах.
```
DELETE FROM customer
WHERE active = 0
```

## Задание 9
Напишите оператор **DDL** для создания таблицы `department`. Таблица должна содержать только два столбца: \
`department_id` — целочисленное поле с автоинкрементом, \
`name` — поле varchar(64), не пустое.

После этого добавьте в таблицу две записи с названиями отделов **Management** и **Sales**.
```
CREATE TABLE department (
    department_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(64) NOT NULL
);

INSERT INTO department (name)
VALUES ('Management'),
       ('Sales');
```

## Задание 10
После создания таблицы `department` в этой задаче измените таблицу `staff`, добавив новый столбец `department_id` в таблицу. \
Столбец `department_id` должен быть целочисленным полем с внешним ключом для столбца `department_id` в таблице `department`. \
Для всех записей в таблице `staff` установите значение в столбце `department_id`, соответствующее отделу **Sales**.
```
ALTER TABLE staff
ADD COLUMN department_id INT;

UPDATE staff
SET department_id = (
    SELECT department_id
    FROM department
    WHERE name = 'Sales'
)
WHERE department_id IS NULL;

ALTER TABLE staff
ADD CONSTRAINT fk_department_id FOREIGN KEY (department_id) REFERENCES department(department_id)
```

## Задание 11
Обновите стоимость аренды фильма и стоимость замены диска для фильмов в категории **NC-17** (фильмы для взрослых). Напишите запрос который увеличивает стоимость проката на **20%**, а стоимость замены на **$5**. \
Выполните все изменения одним запросом.
```
UPDATE film
SET
    rental_rate = rental_rate * 1.2,
    replacement_cost = replacement_cost + 5
WHERE
    rating = 'NC-17'
```

## Задание 12
В адресе клиента **JO FOWLER** обнаружена ошибка. Исправьте её указав верный почтовый индекс **98322**.
```
UPDATE address a

INNER JOIN customer c
ON a.address_id = c.address_id

SET
    a.postal_code = 98322
WHERE
    c.first_name = 'JO'
    AND c.last_name = 'FOWLER'
```

## Задание 13
Выполните коррекцию стоимости аренды фильмов следующим образом: стоимость аренды для фильмов в категории **G** должна быть снижена на **5%**, для фильмов в категориях **R** и **NC-17** (фильмы для взрослых) повышена на **20%**, для всех остальных фильмов стоимость повышается на **5%**. \
Выполните все изменения одним запросом.
```
UPDATE film
SET
    rental_rate = CASE
        WHEN rating = 'G' THEN rental_rate * 0.95
        WHEN rating = 'R' OR rating = 'NC-17' THEN rental_rate * 1.2
        ELSE rental_rate * 1.05
    END
```

## Задание 14
Обновите таблицу `film` так чтобы стоимость замены `replacement_cost` стала равна пятикратной стоимости аренды `rental_rate` округленной вверх до ближайшего целого значения.
```
UPDATE film
SET
    replacement_cost = CEIL(rental_rate*5)
```

