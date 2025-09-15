# Задания к главе 6

### 1
```
select * from bookings.tickets limit 20;
SELECT count( * ) FROM bookings.tickets;
SELECT count( * ) FROM bookings.tickets WHERE passenger_name LIKE '% %'; -- 2 слова в имени, разделенные одним пробелом
SELECT count( * ) FROM bookings.tickets WHERE passenger_name LIKE '% % %'; -- 3 слова 
SELECT count( * ) FROM bookings.tickets WHERE passenger_name LIKE '% %%'; -- 2 слова

```
### 2
```
SELECT passenger_name
	FROM bookings.tickets
	WHERE passenger_name LIKE '___ %';
	
SELECT passenger_name
	FROM bookings.tickets
	WHERE passenger_name LIKE '% _____'; -- пассажиры с фамилиями, состоящими из пяти букв

```
### 3
```
SELECT passenger_name
	FROM bookings.tickets
	WHERE passenger_name SIMILAR TO 'AL%'; -- пассажиры, чьи имена начинаются на AL

SELECT passenger_id
	FROM bookings.tickets
	WHERE passenger_id SIMILAR TO '[4-5][1-2]% 45%'; -- пассажиры, чьи паспортные данные соотвествуют '[4-5][1-2]% 45%'

SELECT airport_name, city
	FROM bookings.airports
	WHERE city SIMILAR TO 'Москва|Сочи'; -- аэропорты, которые находятся в Москве или Сочи

select * from bookings.bookings limit 20;
	
SELECT *
	FROM bookings.bookings
	WHERE book_ref SIMILAR TO '0{3}[6-9][A-D]%';

```
### 4
```
SELECT *
	FROM bookings.bookings
	WHERE total_amount BETWEEN 49000.00 AND 50000.00;


### 5

SELECT COALESCE(NULL, NULL, 'Hello', 'World');  --первое ненулевое значение
SELECT NULLIF(10, 10), NULLIF(10, 20);  --если элементы равны то NULL, иначе первый элемент
SELECT GREATEST(1, 5, 3, 2);  --наибольше значение из списка аргументов
SELECT LEAST(1, 5, 3, 2);  --наименьшее значение из списка аргументов

```
### 6
```
select * from bookings.routes limit 20;
select * from bookings.aircrafts limit 20;

--  Выясните,на каких маршрутах используются самолеты компании Boeing
select distinct flight_no, departure_airport, departure_airport_name, departure_city, arrival_airport, arrival_airport_name, arrival_city, a.model, duration, days_of_week 
	from bookings.routes r
	join bookings.aircrafts a on r.aircraft_code = a.aircraft_code
	where a.model LIKE 'Boeing %';

```
### 7
```
SELECT DISTINCT departure_city, arrival_city
	FROM bookings.routes r
	JOIN bookings.aircrafts a ON r.aircraft_code = a.aircraft_code
	WHERE a.model = 'Boeing 777-300'
	ORDER BY 1;

SELECT DISTINCT 
	GREATEST(departure_city, arrival_city),
	LEAST(departure_city, arrival_city)
	FROM bookings.routes r
	JOIN bookings.aircrafts a ON r.aircraft_code = a.aircraft_code
	WHERE a.model = 'Boeing 777-300'
	ORDER BY 1;

```
### 8
```
SELECT 
    status,
    model
FROM 
    bookings.flights fl
FULL OUTER JOIN 
    bookings.aircrafts a ON a.aircraft_code = fl.aircraft_code limit 20;
```

### 9
```
SELECT departure_city, arrival_city, count( * )
	FROM bookings.routes
	WHERE departure_city = 'Москва'
	AND arrival_city = 'Санкт-Петербург'
	GROUP BY departure_city, arrival_city;


```
### 10
```
--число направлений, по которым летают самолеты из каждого города
SELECT distinct departure_city, count(distinct arrival_city) 
	FROM bookings.routes
	GROUP BY departure_city
	ORDER BY count DESC;

```
### 11
```
SELECT flight_no, departure_airport, departure_airport_name, departure_city, arrival_airport, arrival_airport_name, arrival_city, aircraft_code, duration, days_of_week
	FROM bookings.routes
	WHERE departure_city = 'Москва' and arrival_city = 'Ульяновск'
	LIMIT 20;
	
SELECT arrival_city, sum(array_length(days_of_week, 1)) 
	FROM bookings.routes
	WHERE departure_city = 'Москва'
	GROUP BY arrival_city
	ORDER BY sum DESC
	LIMIT 5;

```
### 13
```
SELECT f.departure_city, f.arrival_city,
	max( tf.amount ), min( tf.amount )
	FROM bookings.flights_v f 
	FULL OUTER JOIN bookings.ticket_flights tf ON f.flight_id = tf.flight_id 
	GROUP BY 1, 2
	ORDER BY 1, 2;
	
```
### 14
```
SELECT right( passenger_name, length(passenger_name) - strpos( passenger_name, ' ' ) ) 
	AS firstname, count( * )
	FROM bookings.tickets 
	GROUP BY 1
	ORDER BY 2 DESC;

```
### 17
```
SELECT a.aircraft_code, a.model, s.fare_conditions, count( * ) AS num
	FROM bookings.aircrafts a
		JOIN bookings.seats s ON a.aircraft_code = s.aircraft_code
	GROUP BY 1, 2, 3
	ORDER BY 1;

```
### 18
```
select * from bookings.routes limit 20;

SELECT 
	   a.aircraft_code AS a_code,
	   a.model,
	   r.aircraft_code AS r_code,
	   count( r.aircraft_code ) AS num_routes,
	   (count( r.aircraft_code )::numeric / (select count(*) from bookings.routes)::numeric)::numeric(4,3) as fraction
	FROM bookings.aircrafts a
	LEFT OUTER JOIN bookings.routes r ON r.aircraft_code = a.aircraft_code
	GROUP BY 1, 2, 3
	ORDER BY 4 DESC;
```

### 21
```
SELECT city
	FROM bookings.airports
		WHERE city <> 'Москва'
	EXCEPT -- Except (в пер. 'исключая')
SELECT arrival_city
	FROM bookings.routes
	WHERE departure_city = 'Москва'
	ORDER BY city;
```

### 22
```
SELECT aa.city, aa.airport_code, aa.airport_name
	FROM (
		SELECT city, count( * )
			FROM bookings.airports
			GROUP BY city
			HAVING count( * ) > 1 -- обязательно, т.к. запрос SELECT city FROM bookings.airports выберет все города
		) AS a
	JOIN bookings.airports AS aa ON a.city = aa.city
	ORDER BY aa.city, aa.airport_name;

SELECT aa.city, aa.airport_code, aa.airport_name
	FROM (
		SELECT city FROM bookings.airports
	) AS a
	JOIN bookings.airports AS aa ON a.city = aa.city
	ORDER BY aa.city, aa.airport_name;
```

### 23
```
SELECT count( * )
	FROM ( SELECT DISTINCT city FROM bookings.airports ) AS a1
	JOIN ( SELECT DISTINCT city FROM bookings.airports ) AS a2
	ON a1.city <> a2.city;
	
WITH a1 AS (
		SELECT DISTINCT city FROM bookings.airports
	),
	a2 AS (
		SELECT DISTINCT city FROM bookings.airports
	)
	SELECT count(*) FROM a1 JOIN a2
	ON a1.city <> a2.city;
```

### 24
```
SELECT * FROM bookings.airports
	WHERE timezone IN ( 'Asia/Novokuznetsk', 'Asia/Krasnoyarsk' );
	
SELECT * FROM bookings.airports
	WHERE timezone = ANY (
		VALUES ( 'Asia/Novokuznetsk' ), ( 'Asia/Krasnoyarsk' )
);

SELECT departure_city, count( * )
	FROM bookings.routes
	GROUP BY departure_city
	HAVING departure_city = ANY(
		SELECT city
 			FROM bookings.airports
	 		WHERE longitude > 150
	)
ORDER BY count DESC;

```