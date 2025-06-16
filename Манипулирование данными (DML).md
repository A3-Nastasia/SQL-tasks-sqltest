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

## Задание 15
Переместить фильм "**CHINATOWN GLADIATOR**" из категории **New** в категорию **Action**.

```
UPDATE film_category fc

INNER JOIN film f
ON fc.film_id = f.film_id

SET
    category_id = (
        SELECT category_id 
        FROM category
        WHERE name = 'Action'
    )
WHERE
    f.title = "CHINATOWN GLADIATOR";
```

## Задание 16
Создайте таблицу `penguins_by_islands` с полями `island` - тип поля текст, первичный ключ и `penguins_count` - тип поля - целое число. \
Заполните новую таблицу данными на основе таблицы `penguins` так, чтобы столбец `island` - содержал название острова, а `penguins_count` - количество проживающих на нём пингвинов.
```
CREATE TABLE penguins_by_islands (
    island VARCHAR(255) PRIMARY KEY,
    penguins_count INT
);

INSERT INTO penguins_by_islands (
    island, 
    penguins_count
)
    SELECT 
        island, 
        COUNT(*) AS penguins_count
    FROM penguins
    GROUP BY island
```

## Задание 17
Для поддержания актуальности таблицы `penguins_by_islands` (созданной в этом задании) необходимо написать триггер, который срабатывает после добавления строк в таблицу `penguins`. \
Код триггера должен обновлять количество пингвинов в соответствующей записи результирующей таблицы или создавать новую запись, если пингвин добавлен для острова, которого ещё нет в таблице.
```
CREATE TRIGGER tr_penguins_insert
AFTER INSERT ON penguins
FOR EACH ROW
BEGIN
    DECLARE island_exists INT DEFAULT 0;
    
    SELECT COUNT(*) INTO island_exists
    FROM penguins_by_islands
    WHERE island = NEW.island;

    IF island_exists = 0 THEN
        INSERT INTO penguins_by_islands (
            island, 
            penguins_count
        )
        VALUES (
            NEW.island, 
            1
        );
    ELSE
        UPDATE penguins_by_islands
        SET
           penguins_count = penguins_count + 1
        WHERE 
            island = NEW.island;
    END IF;
END
```
Вариант для SQLite:
```
CREATE TRIGGER tr_penguins_insert
AFTER INSERT ON penguins
FOR EACH ROW
BEGIN
    UPDATE penguins_by_islands
    SET
       penguins_count = penguins_count + 1
    WHERE 
        island = NEW.island;
    
    INSERT OR IGNORE INTO penguins_by_islands (
        island, 
        penguins_count
    )
    VALUES (
        NEW.island, 
        1
    );
END
```

## Задание 18
Для поддержания актуальности таблицы `penguins_by_islands` (созданной в этом задании) создайте триггер, который срабатывает после удаления строк из таблицы `penguins` и уменьшает количество пингвинов обитающих на соответствующем острове.
```
CREATE TRIGGER tr_penguins_count_delete
AFTER DELETE ON penguins
FOR EACH ROW

BEGIN
    UPDATE penguins_by_islands
    SET
        penguins_count = penguins_count - 1
    WHERE 
        island = OLD.island;
END
```

## Задание 19
Удалите все записи о пингвинах с острова **Biscoe** из таблицы `little_penguins`.
```
DELETE FROM little_penguins
WHERE
    island = 'Biscoe'
```

## Задание 20
Из таблицы `EMPLOYEE_PROJECT` удалите все записи о сотрудниках, работающих над проектом **Marketing project 3**.
```
DELETE e 
FROM EMPLOYEE_PROJECT e

INNER JOIN PROJECT p
ON e.PROJ_ID = p.PROJ_ID

WHERE
    p.PROJ_NAME = 'Marketing project 3'
```

Для Firebind:
```
DELETE FROM EMPLOYEE_PROJECT e
WHERE EXISTS (
    SELECT 1 
    FROM PROJECT p

    WHERE
        p.PROJ_NAME = 'Marketing project 3'
        AND e.PROJ_ID = p.PROJ_ID
)
```

## Задание 21
Из таблицы `film` удалите записи о фильмах, которых нет в таблице `inventory`.
```
DELETE f FROM film f

LEFT JOIN inventory i
ON f.film_id = i.film_id

WHERE i.film_id IS NULL
```

## Задание 22
Переименуйте таблицу `little_penguins` в `small_penguins`.
```
ALTER TABLE little_penguins RENAME TO small_penguins
```

## Задание 23
На основе таблицы `penguins` создайте представление `penguins_averages` с полями `species` - вид пингвинов, `sex` - пол пингвинов, `penguins_count` - количество особей и полями `avg_bill_length_mm`, `avg_bill_depth_mm`, `avg_flipper_length_mm` и `avg_body_mass_g` - представляющими средние значения в разрезе вида и пола животных.
```
CREATE VIEW penguins_averages AS
    SELECT 
        species,
        sex,
        COUNT(*) AS penguins_count,
        AVG(bill_length_mm) AS avg_bill_length_mm,
        AVG(bill_depth_mm) AS avg_bill_depth_mm,
        AVG(flipper_length_mm) AS avg_flipper_length_mm,
        AVG(body_mass_g) AS avg_body_mass_g
        
    FROM penguins
    WHERE sex IS NOT NULL
    GROUP BY species, sex
```

## Задание 24
Для улучшения производительности аналитических запросов создайте индекс с именем `island_ix` на столбце `island` таблицы `penguins`.
```
CREATE INDEX island_ix ON penguins(island)
```
## Задание 25
Для предотвращения дубликатов создайте уникальный индекс с именем `ident_ix` на столбце `ident` таблицы `staff`.
```
CREATE UNIQUE INDEX ident_ix ON staff(ident)
```

## Задание 26
Удалите таблицу `little_penguins`.
```
DROP TABLE little_penguins
```

## Задание 27
Для аналитических целей мы часто запрашиваем таблицу `rental` с условиями, подобными **`DATE_FORMAT(return_date, '%Y-%m')` = 'ГГГГ-ММ'** (где 'ГГГГ-ММ' обозначает конкретный год и месяц). Для значительного улучшения производительности этих запросов создайте функциональный индекс с именем `idx_return_date_func` на столбце `return_date`, используя функцию `DATE_FORMAT`.
```
CREATE INDEX idx_return_date_func ON rental((DATE_FORMAT(return_date, '%Y-%m')))
```