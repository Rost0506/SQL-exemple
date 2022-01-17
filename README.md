# SQL-exemple
Вывести продукты количество которых в продаже меньше самого малого среднего количества продуктов в деталях заказов (группировка по product_id). Результирующая таблица должна иметь колонки product_name и units_in_stock.
'''
1.
Select product_name, units_in_stock
FROM products
Where units_in_stock < All (
Select AVG(quantity) 
FROM order_details
Group By product_id
)
Order by units_in_stock Desc;
'''
