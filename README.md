# SQL-exemple
## Задания делались на основе учебной базы данных northwind
1. Вывести продукты количество которых в продаже меньше самого малого среднего количества продуктов в деталях заказов (группировка по product_id). Результирующая таблица должна иметь колонки product_name и units_in_stock.
```
Select product_name, units_in_stock
FROM products
Where units_in_stock < All (
Select AVG(quantity) 
FROM order_details
Group By product_id
)
Order by units_in_stock Desc;
```
2.Напишите запрос, который выводит общую сумму фрахтов заказов для компаний-заказчиков для заказов, стоимость фрахта которых больше или равна средней величине стоимости фрахта всех заказов, а также дата отгрузки заказа должна находится во второй половине июля 1996 года. Результирующая таблица должна иметь колонки customer_id и freight_sum, строки которой должны быть отсортированы по сумме фрахтов заказов.
```
Select customer_id, AVG (freight) AS freight_avg
From orders
Group By customer_id;

Select customer_id, SUM(freight) AS freight_sum
From orders AS o
Inner Join (Select customer_id, AVG (freight) AS freight_avg
            From orders
            Group By customer_id) AS oa
Using(customer_id)
Where freight > freight_avg AND shipped_date Between '1996-07-16' ANd '1996-07-31'
Group By customer_id
Order by freight_sum;
```
3.Напишите запрос, который выводит 3 заказа с наибольшей стоимостью, которые были созданы после 1 сентября 1997 года включительно и были доставлены в страны Южной Америки. Общая стоимость рассчитывается как сумма стоимости деталей заказа с учетом дисконта. Результирующая таблица должна иметь колонки customer_id, ship_country и order_price, строки которой должны быть отсортированы по стоимости заказа в обратном порядке.
```
Select customer_id, ship_country, order_price
From orders 
Join (Select order_id, SUM (unit_price * quantity - unit_price * quantity * discount) AS order_price
      From order_details
      Group By order_id) AS od
Using (order_id)
Where ship_country IN ('Bolivia', 'Brazil','Chile')
And order_date >= '1997-09-01'
Order By order_price Desc
Limit 3;
```
4.Вывести все товары (уникальные названия продуктов), которых заказано ровно 10 единиц (конечно же, это можно решить и без подзапроса
```
Select product_name
From products
Where product_id = ANY (
Select product_id From order_details Where quantity =10
);
```
