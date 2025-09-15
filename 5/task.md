# Задания к главе 7,8

```
CREATE TEMP TABLE aircrafts_tmp AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_tmp
	ADD PRIMARY KEY ( aircraft_code );
ALTER TABLE aircrafts_tmp
	ADD UNIQUE ( model );
	
CREATE TEMP TABLE aircrafts_log AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_log
	ADD COLUMN when_add timestamp;
ALTER TABLE aircrafts_log
	ADD COLUMN operation text;

SELECT * FROM aircrafts_tmp ORDER BY model;
SELECT * FROM aircrafts_log ORDER BY model;

```
### 1
```
CREATE TEMP TABLE aircrafts_tmp AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_tmp
	ADD PRIMARY KEY ( aircraft_code );
ALTER TABLE aircrafts_tmp
	ADD UNIQUE ( model );

DROP TABLE aircrafts_log;

CREATE TEMP TABLE aircrafts_log AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;

ALTER TABLE aircrafts_log
	ADD COLUMN when_add timestamp;
ALTER TABLE aircrafts_log
	ADD COLUMN operation text;

ALTER TABLE aircrafts_log
	ALTER COLUMN when_add SET DEFAULT current_timestamp;

INSERT INTO aircrafts_tmp
	SELECT * FROM bookings.aircrafts;

WITH add_row AS( 
		SELECT * FROM bookings.aircrafts
	)
	INSERT INTO aircrafts_log (aircraft_code, model, range, specifications, operation)
	SELECT add_row.aircraft_code, add_row.model, add_row.range, add_row.specifications, 'INSERT'
 	FROM add_row;	

SELECT * FROM aircrafts_log;
SELECT * FROM aircrafts_tmp;

```
### 2
```
CREATE TEMP TABLE aircrafts_tmp2 AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_tmp2
	ADD PRIMARY KEY ( aircraft_code );
ALTER TABLE aircrafts_tmp2
	ADD UNIQUE ( model );

CREATE TEMP TABLE aircrafts_log2 AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_log2
	ADD COLUMN when_add timestamp;
ALTER TABLE aircrafts_log2
	ADD COLUMN operation text;

WITH add_row AS( 
	INSERT INTO aircrafts_tmp2
		SELECT * FROM bookings.aircrafts
		RETURNING aircraft_code, model, range, specifications, current_timestamp, 'INSERT'
	)
	INSERT INTO aircrafts_log2
	SELECT aircraft_code, model, range, specifications, current_timestamp, 'INSERT' FROM add_row;

SELECT * FROM aircrafts_log2;

```
### 3
```
CREATE TEMP TABLE aircrafts_tmp3 AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_tmp3
	ADD PRIMARY KEY ( aircraft_code );
ALTER TABLE aircrafts_tmp3
	ADD UNIQUE ( model );

INSERT INTO aircrafts_tmp3 SELECT * FROM bookings.aircrafts RETURNING *;

```
### 4
```
DROP TABLE seats_tmp4;

CREATE TEMP TABLE seats_tmp4
 ( LIKE bookings.seats INCLUDING CONSTRAINTS INCLUDING INDEXES );

SELECT * FROM bookings.seats limit 10;

INSERT INTO seats_tmp4 
	VALUES ('319', '2A', 'Business')
	ON CONFLICT (aircraft_code, seat_no)
	DO UPDATE SET 
		aircraft_code = excluded.aircraft_code,
		seat_no = excluded.seat_no,
		fare_conditions = excluded.fare_conditions
	RETURNING *;

INSERT INTO seats_tmp4 
	VALUES ('319', '2A', 'Business')
	ON CONFLICT ON CONSTRAINT seats_tmp4_pkey
	DO UPDATE SET 
		aircraft_code = excluded.aircraft_code,
		seat_no = excluded.seat_no,
		fare_conditions = excluded.fare_conditions
	RETURNING *;
```

### 5
```
INSERT INTO seats_tmp4 
	VALUES ('319', '2A', 'Business')
	ON CONFLICT (aircraft_code, seat_no)
	DO UPDATE SET fare_conditions = excluded.fare_conditions
	WHERE seats_tmp4.fare_conditions <> excluded.fare_conditions
	RETURNING *;

SELECT * FROM seats_tmp4 limit 10;

```
### 6
```
CREATE TEMP TABLE aircrafts_tmp6 AS
	SELECT * FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_tmp6
	ADD PRIMARY KEY ( aircraft_code );
ALTER TABLE aircrafts_tmp6
	ADD UNIQUE ( model );

COPY aircrafts_tmp6 FROM STDIN WITH ( FORMAT csv );
-- смещение записей происходит из-за пробела после запятой
SELECT * FROM aircrafts_tmp6;

```
### 7
```
-- путь к файлу: /home/student/test.csv

CREATE TEMP TABLE aircrafts_tmp7 AS
	SELECT aircraft_code, model, range FROM bookings.aircrafts WITH NO DATA;
	
ALTER TABLE aircrafts_tmp7
	ADD PRIMARY KEY ( aircraft_code );
ALTER TABLE aircrafts_tmp6
	ADD UNIQUE ( model );

COPY aircrafts_tmp7
	FROM '/home/student/test.csv' WITH ( FORMAT csv );

SELECT * FROM aircrafts_tmp7;
```
---------------------------------------------------

# Глава 8. Индексы

### 1
-- Ошибки не будет, поскольку значения NULL не равны никаким другим значениям, в т.ч. и NULL
-- Если уникальный индекс содержит несколько полей, то они будут равны только в том случае, 
-- когда соответсвущие поля равны друг другу 

### 2
```
SELECT count( * )
	FROM bookings.tickets
	WHERE passenger_name = 'IVAN IVANOV';

CREATE INDEX
	ON bookings.tickets (passenger_name);
--Запрос обрабатывается быстрее, поскольку команда сохраняется в кэш и при следующем ее вызове значения берутся из кэша
```
### 3
```
SELECT count( * ) -- 131,25 мс  INDEX: 80 мс
	FROM bookings.ticket_flights
 	WHERE fare_conditions = 'Comfort';

SELECT count( * ) -- 135,75 мс  INDEX: 118,75 мс
	FROM bookings.ticket_flights
 	WHERE fare_conditions = 'Business';
	
SELECT count( * ) -- 151 мс  INDEX: 167,5 мс
	FROM bookings.ticket_flights
 	WHERE fare_conditions = 'Economy';

CREATE INDEX
	ON bookings.ticket_flights (fare_conditions);
```

### 4
```
SELECT * FROM bookings.flights limit 10;

CREATE INDEX flights_index
	ON bookings.flights (
		departure_airport ASC NULLS LAST, 
		scheduled_departure DESC NULLS FIRST
	);

DROP INDEX bookings.flights_index;

SELECT departure_airport, scheduled_departure
	FROM bookings.flights
	ORDER BY scheduled_departure DESC;
-- СВВ: без индекса 90 мс, с индексом 75 мс.

```
### 9
```
CREATE INDEX tickets_pass_name
	ON bookings.tickets ( passenger_name text_pattern_ops );

SELECT count(*) FROM bookings.tickets;
```