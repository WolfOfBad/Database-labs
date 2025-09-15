# Задания к главе 5

### 1
```
CREATE TABLE students
	( record_book numeric( 5 ) NOT NULL,
	name text NOT NULL,
	doc_ser numeric( 4 ),
	doc_num numeric( 6 ),
	who_adds_row text DEFAULT current_user, -- добавленный столбец
	PRIMARY KEY ( record_book )
);

ALTER TABLE students
	ADD COLUMN adding_time timestamp DEFAULT current_timestamp;

INSERT INTO students ( record_book, name, doc_ser, doc_num )
	VALUES ( 12300, 'Иванов Иван Иванович', 0402, 543281 );
INSERT INTO students ( record_book, name, doc_ser, doc_num )
	VALUES ( 77777, 'Егоров Егор Владимирович', 0402, 543281 );

select * from students;

```

### 2
```
drop table progress;
CREATE TABLE progress
	( record_book numeric( 5 ) NOT NULL,
	subject text NOT NULL,
	acad_year text NOT NULL,
	term numeric( 1 ) NOT NULL CHECK ( term = 1 OR term = 2 ),
	mark numeric( 1 ) NOT NULL CHECK ( mark >= 3 AND mark <= 5 )
	DEFAULT 5,
	FOREIGN KEY ( record_book )
	REFERENCES students ( record_book )
	ON DELETE CASCADE
	ON UPDATE CASCADE
);

select * from progress;

alter table progress
	add column test_form text NOT NULL CHECK ( test_form = 'экзамен' or test_form = 'зачет');

ALTER TABLE progress
	ADD CHECK (
	( test_form = 'экзамен' AND mark IN ( 3, 4, 5 ) )
	OR
	( test_form = 'зачет' AND mark IN ( 0, 1 ) )
);

ALTER TABLE progress
	drop constraint progress_mark_check;


INSERT INTO progress
	VALUES ( 77777, 'Базы данных', '2024', 1, 1, 'зачет');
INSERT INTO progress
	VALUES ( 77777, 'РиАТ', '2024', 1, 5, 'экзамен');

-- Ошибка, поскольку работает ограничение progress_mark_check;
INSERT INTO progress
	VALUES ( 12300, 'ООП', '2024', 2, 3, 'зачет');

select * from progress;

```
### 3
```
INSERT INTO progress 
	VALUES ( 12300, 'ООП', '2024', 2, null, 'зачет');
-- Ограничение NOT NULL не является избыточным, поскольку CHECK допускает присвоение NULL	

select * from progress;

```
### 4
```
drop TABLE progress2;
CREATE TABLE progress2
	( record_book numeric( 5 ) NOT NULL,
	subject text NOT NULL,
	acad_year text NOT NULL,
	term numeric( 1 ) NOT NULL CHECK ( term = 1 OR term = 2 ),
	mark numeric( 1 ) NOT NULL CHECK ( mark >= 3 AND mark <= 5 )
	DEFAULT 6,
	FOREIGN KEY ( record_book )
	REFERENCES students ( record_book )
	ON DELETE CASCADE
	ON UPDATE CASCADE
);

-- Ошибка default значения выходит на этапе вставки строки в таблицу
INSERT INTO progress2 ( record_book, subject, acad_year, term )
	VALUES ( 12300, 'Физика', '2016/2017',1 );
INSERT INTO progress2
	VALUES ( 12300, 'Физика', '2016/2017',1, 4);

```
### 5
```
SELECT (null = null); --NULL-значения не считаются совпадающими

drop table students5;
CREATE TABLE students5
	( record_book numeric( 5 ) NOT NULL,
	name text NOT NULL,
	doc_ser numeric( 4 ),
	doc_num numeric( 6 ),
	who_adds_row text DEFAULT current_user,
	PRIMARY KEY ( record_book )
);

alter table students5
	add constraint unique_doc_ser_doc_num unique (doc_ser, doc_num);

insert into students5
	values (12345, 'Ivanov Ivan', 9424, 720419);
insert into students5
	values (12345, 'Ivanov Ivan', 9418, null);
insert into students5
	values (98765, 'Egorov Egor', 9418, null);
insert into students5
	values (01010, 'Grabalin Ivan', null, null);
insert into students5
	values (11122, 'Voronov Vova', null, null);
	
select * from students5;	
	
```
### 6
```
CREATE TABLE students6
	( record_book numeric( 5 ) NOT NULL UNIQUE,
	name text NOT NULL,
	doc_ser numeric( 4 ),
	doc_num numeric( 6 ),
	PRIMARY KEY ( doc_ser, doc_num )
);

CREATE TABLE progress6
	( doc_ser numeric( 4 ),
	doc_num numeric( 6 ),
	subject text NOT NULL,
	acad_year text NOT NULL,
	term numeric( 1 ) NOT NULL CHECK ( term = 1 OR term = 2 ),
	mark numeric( 1 ) NOT NULL CHECK ( mark >= 3 AND mark <= 5 )
	DEFAULT 5,
	FOREIGN KEY ( doc_ser, doc_num )
	REFERENCES students6 ( doc_ser, doc_num )
	ON DELETE CASCADE
	ON UPDATE CASCADE
);

select * from students6;

insert into students6
	values (11111, 'Voronov Vova', 9424, 720419);
insert into students6
	values (22222, 'Egorov Egor', 9418, 341705);

select * from progress6;

INSERT INTO progress6
	VALUES ( 9424, 720419, 'Физика', '2023/2024', 1, 5);
INSERT INTO progress6
	VALUES ( 9418, 341705, 'ООП', '2023/2024', 1, 5);
```
### 8
```
create table subjects(
	subject_id integer primary key,
	subject text unique
);

insert into subjects 
	values (1, 'Математика');
insert into subjects 
	values (2, 'ООП');
insert into subjects 
	values (3, 'Физика');
	
select * from subjects;
select * from progress6;

ALTER TABLE progress6
 ALTER COLUMN subject SET DATA TYPE integer
 	USING ( CASE WHEN subject = 'ООП' THEN 2
 				 WHEN subject = 'Физика' THEN 3
END );

ALTER TABLE progress6
 RENAME column subject to subject_id;

select * from progress6;

ALTER TABLE progress6 
	ADD CONSTRAINT fk_subject FOREIGN KEY (subject_id) REFERENCES subjects(subject_id);

```
### 9
```
INSERT INTO students6 ( record_book, name, doc_ser, doc_num )
 VALUES ( 12300, '', 0402, 543281 );

delete from students6
	where name = '';

select * from students6;

ALTER TABLE students6 ADD CHECK ( name <> '' );
INSERT INTO students6 VALUES ( 12346, '     ', 0406, 112233 );
INSERT INTO students6 VALUES ( 12348, 'Egor Egorov   ', 0407, 112234 );
INSERT INTO students6 VALUES ( 12349, '  Egor Egorov   ', 0407, 112234 );
INSERT INTO students6 VALUES ( 12350, '    ', 0407, 112234 );

delete from students6
	where trim(' ' from name) = '';

SELECT *, length( name ) FROM students6;
ALTER TABLE students6 ADD CHECK (length(trim(both ' ' from name)) <> 0);

```
### 10
```
ALTER TABLE students6
 ALTER COLUMN doc_ser SET DATA TYPE character(4);
```
### 12
```
ALTER TABLE progress RENAME TO progress_new;

```
### 13 
```
DROP TABLE bookings.airports;
```
### 13
```
DROP TABLE bookings.airports;
```
### 14
```
UPDATE siberian_airports
SET aircraft_code='773'
WHERE arrival_airport= 'KEJ';
```
### 16
```
UPDATE siberian_airports
SET aircraft_code='773'
WHERE arrival_airport= 'KEJ';
```
### 17
```
CREATE VIEW airports_names AS
SELECT airport_code, airport_name, city
FROM bookings.airports;
SELECT * FROM airports_names;


CREATE VIEW siberian_airports AS
SELECT * FROM bookings.airports
WHERE city = 'Новосибирск' OR city = 'Кемерово';
SELECT * FROM siberian_airports;

DROP VIEW siberian_flighties;
CREATE VIEW siberian_flighties AS
SELECT * FROM bookings.flights
WHERE arrival_airport = 'KEJ' OR arrival_airport = 'OVB';
SELECT * FROM siberian_flighties;	
		

```
### 18
ALTER TABLE bookings.aircrafts ADD COLUMN specifications jsonb;

UPDATE bookings.aircrafts
SET specifications =
'{ "crew": 2,
"engines": { "type": "IAE V2500",
"num": 2
}
}'::jsonb
WHERE aircraft_code = '320';

SELECT model, specifications
FROM bookings.aircrafts
WHERE aircraft_code = '320';


SELECT model, specifications->'engines' AS engines
FROM bookings.aircrafts
WHERE aircraft_code = '320';


SELECT model, specifications #> '{ engines, type }'
FROM bookings.aircrafts
WHERE aircraft_code = '320';

SELECT * FROM bookings.airports

ALTER TABLE bookings.airports ADD COLUMN details jsonb;


UPDATE bookings.airports
SET details =
'{ "score": 3,
"district": "Мирнинский район",
"Passenger traffic": 291400
}'::jsonb
WHERE airport_name = 'Мирный';

SELECT airport_name, details
FROM bookings.airports
WHERE airport_name = 'Мирный';

SELECT airport_name, details->'district' as district
FROM bookings.airports
WHERE airport_name = 'Мирный';

SELECT airport_name, details->'score' as score
FROM bookings.airports
WHERE airport_name = 'Мирный';

```

