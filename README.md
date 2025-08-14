# Домашнее задание к занятию «SQL. Часть 2»
### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

### Решение:

```
SELECT CONCAT(s.first_name , " ", s.last_name) AS 'Сотрудник магазина', cm.city AS 'Город нахождения магазина', COUNT(c.customer_id) AS 'Количество покупателей'
FROM staff AS s
JOIN address AS a ON a.address_id = s.address_id
JOIN city AS cm ON cm.city_id = a.city_id
JOIN store AS st ON st.store_id = s.store_id
JOIN customer AS c ON c.store_id = s.store_id
GROUP BY staff_id
HAVING COUNT(c.customer_id) > 300;
```

![png](image-5.png)

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение:

```
SELECT COUNT(length) AS 'Количество фильмов больше средней продолжительности'
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```
![png](image-4.png)

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

### Решение:

```
select	t.amount_of_payments,
	t.month_of_payments,
	(select count(r.rental_id)
	from sakila.rental r
	where DATE_FORMAT(r.rental_date, '%M %Y') = t.month_of_payments) 'count_of_rent'
from (
  select SUM(p.amount) 'amount_of_payments', DATE_FORMAT(p.payment_date, '%M %Y') 'month_of_payments' 
  from sakila.payment p 
  group by DATE_FORMAT(p.payment_date, '%M %Y')) t
order by t.amount_of_payments desc  
limit 1;
```
![png](image-6.png)