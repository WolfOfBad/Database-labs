# Задания к главе 9, 10

create table aircrafts_tmp
	as select * from bookings.aircrafts;
drop table aircrafts_tmp;

--первый терминал

### 1 
```
begin;
	select * from aircrafts_tmp
		for update;
end;

begin;
	lock table aircrafts_tmp
		in access exclusive mode;
end;
```

### 2
```
begin;
	SELECT *
 		FROM aircrafts_tmp
 		WHERE range < 2000;
		
	UPDATE aircrafts_tmp
 		SET range = 2100
 		WHERE aircraft_code = 'CN1';
		
	UPDATE aircrafts_tmp
 		SET range = 1900
 		WHERE aircraft_code = 'CR2';
end;
rollback;

select * from aircrafts_tmp
	order by range;

```
### 4
```
begin;
	SELECT *
 		FROM aircrafts_tmp
 		WHERE range > 6000;
end;
```


### 5
```
begin;
	--являлось подмножеством множества строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
	for update;
	
	--являлось надмножеством множества строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
		where range between 4000 and 6000
		for update;

	--пересекалось с множеством строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
		where range between 2000 and 6000
		for update;
	
	--не пересекалось с множеством строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
		where range between 2000 and 6000
		for update;
	
end;


--второй терминал

### 1
begin;
	select * from aircrafts_tmp
		for update;
end;

```
### 2
```
begin;
	SELECT *
 		FROM aircrafts_tmp
 		WHERE range < 2000;
	
	DELETE FROM aircrafts_tmp WHERE range < 2000;
	
end;
```

### 4
```
begin;
	UPDATE aircrafts_tmp
 		SET range = 6100
 		WHERE aircraft_code = '320';
		
	insert into aircrafts_tmp
		values('322', 'AirbusTemp', 30000, null);
end;

```
### 5
```

begin;
	--являлось подмножеством множества строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
		where range between 4000 and 6000
		for update;
	
	--являлось надмножеством множества строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
	for update;
	
	--пересекалось с множеством строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
		where range between 3000 and 8000
		for update;
	
	--не пересекалось с множеством строк, выбираемых в первой транзакции
	select * from aircrafts_tmp
		where range > 7000
		for update;
	
end;



```

### 1
```
EXPLAIN
	SELECT *
	FROM bookings.bookings
	ORDER BY book_ref;
```
### 2
```
EXPLAIN
	SELECT *
	FROM bookings.ticket_flights
	order by fare_conditions;
	
EXPLAIN
	SELECT *
	FROM bookings.tickets
	order by passenger_name;
```
### 3
```
explain
	with city_from as (
		select distinct city 
			from bookings.airports
	)
select count(*)
	from city_from as a1
	join city_from as a2
	on a1.city <> a2.city;
```
### 4
```
EXPLAIN
	SELECT total_amount FROM bookings.bookings
	ORDER BY total_amount DESC
	LIMIT 5;
```
### 5
```
EXPLAIN
	SELECT city, count( * )
	FROM bookings.airports
	GROUP BY city
	HAVING count( * ) > 1;
```