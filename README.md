## SQL-запросы для приложения "Яндекс.Прилавок".
Яндекс.Прилавок — приложение для заказа продуктов.
Приложение использует базу данных. БД — PostgreSQL. Приложение взаимодействует с БД через npm-пакет `sequelize`  поверх пакета `pg` , `sequelize`  — ORM для работы с различными БД в node.js. Запросы логируются через модуль `winston`.
Приложение отвечает требованиям REST.

# Задачи и проделанная работа:
1. Посчитать, сколько пользователей зарегистрировано в системе. Это таблица user_model. В результате вывести только количество пользователей.
Запрос к БД:
```
SELECT COUNT(id)
FROM user_model;
```

2. Добавить три новых разных продукта в таблицу product_model.
Запрос к БД:
```
INSERT INTO product_model
VALUES (114, 'milk Parmalat', 100, 1, 1, 12), (115, 'persimmon', 110, 0.5, 1, 6), (116, 'coffee Jacobs Monarch', 550, 0.1, 1, 1);
```

4. Посчитать количество продуктов в каждой категории и вывести id только тех категорий, в которых количество продуктов больше пяти. Это таблица product_model. Результат отсортировать в порядке возрастания количества продуктов.
Запрос к БД:
```
SELECT COUNT(id), "categoryId"
FROM product_model
GROUP BY "categoryId"
HAVING COUNT(id) > 5
ORDER BY COUNT(id) ASC;
```

6. Написать запрос, который будет выводить в системе id всех заказов и возможность внести правки. Название этой колонки "update_order". Если статус заказа позволяет вносить изменения, то в колонку "update_order" нужно вывести "yes". Если правки внести нельзя — вывести "no".
Запрос к БД:
```
SELECT id, 'update_order',
CASE
WHEN "deliveryPrice" >500 AND status=0 OR status=1 THEN 'update_order'='yes'
END
FROM order_model;
```

8. Вывести информацию о продуктах, цена которых находится в диапазоне от 200 до 500. Информация по каждому продукту включает: название продукта, цену, название категории, к которой он относится.
Запрос к БД:
```
SELECT pdm.name AS product_name, pdm.price, cgm.name AS category_name
FROM product_model AS pdm
LEFT OUTER JOIN category_model AS cgm ON pdm."categoryId"=cgm.id
WHERE pdm.price BETWEEN 200 AND 500;
```

10. Для каждой карточки вывести ее название и количество продуктов ("productsCount") для этой карточки. Результат отсортировать по названию карточки.
Запрос к БД:
```
SELECT cm.name AS card_name, SUM("productsCount")
FROM kit_model AS km
LEFT OUTER JOIN card_model AS cm ON km."cardId"=cm.id
GROUP BY card_name
ORDER BY card_name;
```

## Инструменты:
<p align="left">
  <a href="https://cygwin.com/" target="_blank" rel="noreferrer"><img src="https://github.com/user-attachments/assets/8b61e328-84bc-40e5-a4be-1eefb49e7b8e" width="36" height="36" alt="Cygwin" /></a>
</p> 
