```

--FUNCTION

CREATE FUNCTION author_name(
    last_name text,
    first_name text,
    middle_name text
) RETURNS text
AS $$ select last_name || ' ' ||
       left(first_name, 1) || '.' ||
       CASE WHEN middle_name != '' -- подразумевает NOT NULL
           THEN ' ' || left(middle_name, 1) || '.'
           ELSE ''
       END;
$$ LANGUAGE sql IMMUTABLE;

drop function author_name;

select author_name(
	'Egorov',
	'Egor',
	'Vladimirovich'
);

select author_name(
	last_name=>'Egorov',
	first_name=>'Egor',
	middle_name=>'Vladimirovich'
);


--PROCEDURE

create table a_tmp
	as select * from bookings.aircrafts;
drop table a_tmp;

select * from a_tmp;

insert into a_tmp
	values('CN3', 'Cessna 208 Caravan', '1200', null);
insert into a_tmp
	values('311', 'Airbus A319-100', '6700', null);

create or replace procedure delete_dups()
language sql
as $$
	delete from a_tmp
		where aircraft_code IN (
			select a2.aircraft_code from a_tmp a1, a_tmp a2
				where a1.model=a2.model and a1.aircraft_code<a2.aircraft_code
		)
$$;

call delete_dups();


--TRIGGER

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

insert into users
	values(1, 'Egor', '1@mail.ru');
insert into users
	values(2, 'Dima', '1@mail.ru');
insert into users
	values(3, 'Artem', '1@mail.ru');

CREATE OR REPLACE FUNCTION update_timestamp()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;


CREATE TRIGGER update_user_timestamp
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION update_timestamp();


UPDATE users SET name = 'Ivan' WHERE id = 1;

select * from users;

```