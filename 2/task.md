# Задания к главе 4

## 1 задание

Ошибку вызовет первый запрос, так как `999.9999` округлится до `1000.00`,
и количество цифр выйдет за предел типа данных. Остальные запросы выполнятся успешно

![image](https://github.com/user-attachments/assets/411d5cb8-8a65-4da8-952c-9307cf5a7031)

Пример запроса вызывающего ошибку

```sql
INSERT INTO test_numeric
VALUES ( 123456, 'Ошибка' );
```

![image](https://github.com/user-attachments/assets/06e5a48c-f9d0-4733-a95f-8569f8b1821e)

## 2 задание

```sql
INSERT INTO test_numeric
VALUES ( 1234567890.0987654321, 'Точность 20 знаков, масштаб 10 знаков' );

INSERT INTO test_numeric
VALUES ( 1.5, 'Точность 2 знака, масштаб 1 знак' );

INSERT INTO test_numeric
VALUES ( 0.12345678901234567890, 'Точность 21 знак, масштаб 20 знаков' );

INSERT INTO test_numeric
VALUES ( 1234567890, 'Точность 10 знаков, масштаб 0 знаков (целое число)' );
```

Делаем запрос и получаем все значения

```sql
SELECT * FROM test_numeric;
```

![image](https://github.com/user-attachments/assets/f6dd44b9-4426-446d-ab09-7a007eaee09f)

## 3 задание

```sql
SELECT 'NaN'::numeric > 10000;
```

![image](https://github.com/user-attachments/assets/ab31003a-307c-41fb-b9b7-45af69764716)

## 4 задание

```sql
SELECT '5e-324'::double precision > '4e-324'::double precision;
```

![image](https://github.com/user-attachments/assets/9b434728-72af-40c3-a704-a6ace545cf2d)

## 5 задание

```sql
SELECT 'Inf'::double precision > 1E+308;
```

![image](https://github.com/user-attachments/assets/c11fe70b-d643-49e5-b22e-e2a6a499cc37)

```sql
SELECT '-Inf'::double precision < 1E-307;
```

![image](https://github.com/user-attachments/assets/d2605f78-1439-43c8-9f20-ee2a1ffab720)

## 6 задание

```sql
SELECT 0.0 * 'Inf'::real;
```

![image](https://github.com/user-attachments/assets/bfaf6faf-fe53-40c9-ac77-d330a2681163)

```sql
select 'NaN'::real > 'Inf'::real;
```

![image](https://github.com/user-attachments/assets/e1fce92e-a6e4-47b4-b8b3-af1e81eaed72)

## 7 задание

```sql
INSERT INTO test_serial ( name ) VALUES ( 'Вишневая' );
INSERT INTO test_serial ( name ) VALUES ( 'Грушевая' );
INSERT INTO test_serial ( name ) VALUES ( 'Зеленая' );

SELECT * FROM test_serial;
```

![image](https://github.com/user-attachments/assets/f1f4ff31-0379-47f7-896d-18cc2b6e0f6c)

После добавления новых строк

```sql
INSERT INTO test_serial ( id, name ) VALUES ( 10, 'Прохладная' );
INSERT INTO test_serial ( name ) VALUES ( 'Луговая' );

SELECT * FROM test_serial;
```

![image](https://github.com/user-attachments/assets/7ac84aa8-98ab-4eb8-89c5-ff1538ef8dd3)

## Задание 8

Добавляем одну строку

```sql
INSERT INTO test_serial ( name ) VALUES ( 'Вишневая' );

SELECT * FROM test_serial;
```

![image](https://github.com/user-attachments/assets/f09b46f3-558f-475f-9640-a1167e8c1ec4)

Добваляем еще строку

```sql
INSERT INTO test_serial ( id, name ) VALUES ( 2, 'Прохладная' );

SELECT * FROM test_serial;
```

![image](https://github.com/user-attachments/assets/87903066-b71c-496a-a433-13f8f0dfbb6e)

Пытаемся добавить еще одну

```sql
INSERT INTO test_serial ( name ) VALUES ( 'Грушевая' );
```

![image](https://github.com/user-attachments/assets/963d3ca3-fec4-4308-a8bc-1f2873604219)

Снова пытаемся добавить ее же, и она добавляется

```sql
INSERT INTO test_serial ( name ) VALUES ( 'Грушевая' );

SELECT * FROM test_serial;
```

![image](https://github.com/user-attachments/assets/ef116894-4c6c-40b9-bd2a-d13c0c73b73a)

Так произошло, потому что мы руками добавили строку у которой был `id` равный `2`, а потом при первом запросе СУБД попыталась создать новую строку у которой также был бы `id` равный `2`.
Но `id` является `PRIMARY KEY` из за чего выскакивает ошибка. При следующем добавлении этой же строки автоматически уже присвоилось `id` равное `3`

Добавляем строку, удаляем ее и добавляем еще одну

```sql
INSERT INTO test_serial ( name ) VALUES ( 'Зеленая' );

DELETE FROM test_serial WHERE id = 4;

INSERT INTO test_serial ( name ) VALUES ( 'Луговая' );

SELECT * FROM test_serial;
```

![image](https://github.com/user-attachments/assets/f2d538d1-c9b3-4f16-89ad-e17742b4d1b2)

## 9 задание

[Григорианский](https://www.postgresql.org/docs/current/datetime-units-history.html#:~:text=PostgreSQL%20follows%20the%20SQL%20standard's,that%20calendar%20was%20in%20use.)

## 10 задание

![image](https://github.com/user-attachments/assets/daf259bf-36dc-47c2-9ed1-17abe8550b1d)

Такие ограничения, поскольку даты и время хранятся в определенном количестве байт и имеют определенный шаг

## 11 задание

```sql
SELECT current_time
```

![image](https://github.com/user-attachments/assets/0ea0bcb2-3ac6-4d6b-9401-d929a390c6db)

```sql
SELECT current_time::time(0)
```

![image](https://github.com/user-attachments/assets/69e96063-64a5-4fff-b02d-6a35d060a7a9)

```sql
SELECT current_time::time(3);
```

![image](https://github.com/user-attachments/assets/9fd2cf04-6770-4d87-968a-e4d307f7a080)

Тоже самое с `timestamp` и `interval`

```sql
SELECT current_timestamp(), current_timestamp()::timestamp(3)

SELECT (current_timestamp - '2004-06-04'::timestamp)::interval, (current_timestamp - '2004-06-04'::timestamp)::interval(3)
```

![image](https://github.com/user-attachments/assets/2bbd5a4e-abdf-43a3-ab5f-d8b98aebaf34)

![image](https://github.com/user-attachments/assets/aab35275-633c-467f-a033-0df87f8ac470)

Поскольку тип `date` имеет шаг в 1 день и у него нету дробной части, то и смысла округлять целые числа нету.

## 12 задание

Я использую контейнер PostgreSQL с Dockerhub, так что изначально тут американский формат времени `MDY`

```sql
SHOW datestyle;
```

![image](https://github.com/user-attachments/assets/484571f7-8d4a-41a1-a52b-8e6f02a8e5de)

```sql
SELECT '05-18-2016'::date;

SELECT '18-05-2016'::date;

SELECT '2016-05-18'::date;
```

![image](https://github.com/user-attachments/assets/8d432daf-6cb1-4a0d-a90a-c93850da24e0)

![image](https://github.com/user-attachments/assets/d48a1b05-8f01-4488-8cc0-117f0dd5bbac)

![image](https://github.com/user-attachments/assets/b9faa2ef-6bdc-44d2-8cc0-556c0bc40484)

```sql
SET datestyle TO 'DMY';

SELECT '18-05-2016'::date;

SELECT '05-18-2016'::date;
```

![image](https://github.com/user-attachments/assets/b48b73e9-7896-44b0-9b59-36c24376e333)

![image](https://github.com/user-attachments/assets/9fe7d53a-0229-4799-beee-3c86f7466240)

```sql
SET datestyle TO DEFAULT;

SELECT '18-05-2016'::timestamp;

SELECT '05-18-2016'::timestamp;
```

![image](https://github.com/user-attachments/assets/bf57de45-5072-4985-bf94-1846798d954b)

![image](https://github.com/user-attachments/assets/4132cc35-f101-4066-b822-05df760f152f)

```sql
SET datestyle TO 'Postgres, DMY';

SHOW datestyle;
```

![image](https://github.com/user-attachments/assets/86283fee-f2b6-4ada-ae83-868799de13fe)

![image](https://github.com/user-attachments/assets/ed199388-ed93-4df2-a925-2553b89bd084)

![image](https://github.com/user-attachments/assets/2d56a5b0-3c26-4dc6-b2bd-8780391993bc)

## 13 задание

Заходим в контейнер, вводим
```cmd
export PGDATESTYLE="postgres"
```

## 14 задание

```cmd
vi /var/lib/postgresql/data/postgres.conf
```

## 15 задание

```sql
SELECT to_char( current_timestamp, 'dd-yyyy-hh-ss-mi-mm' );
```

![image](https://github.com/user-attachments/assets/2db1236f-e2fc-494a-873d-e415772a180c)

## 16 задание

```sql
SELECT 'Feb 29, 2015'::date;
```

![image](https://github.com/user-attachments/assets/72c3bbfa-2851-42d8-a664-03244294f494)

## 17 задание

```sql
SELECT '21:15:16:22'::time;
```

![image](https://github.com/user-attachments/assets/6cb183ad-1de2-47dc-8eaf-bd5a38dd3dfc)

## 18 задание

```sql
SELECT ( '2016-09-16'::date - '2016-09-01'::date );
```

![image](https://github.com/user-attachments/assets/cf17392d-77d7-465a-840d-935e89e6ad85)

Получаем `integer` (количество дней между датами)

## 19 задание

```sql
SELECT ( '20:34:35'::time - '19:44:45'::time );
```

![image](https://github.com/user-attachments/assets/384197f3-504f-461d-95ac-31b6e03bbb95)

```sql
SELECT ( '20:34:35'::time + '19:44:45'::time );
```

![image](https://github.com/user-attachments/assets/94148235-1029-4a22-b0da-aa0912267b05)

При сложении выдаст ошибку. Допустимые операции:

![image](https://github.com/user-attachments/assets/e3f595ee-d51b-4b00-ae1d-c8ed63af303d)

## 20 задание

```sql
SELECT ( current_timestamp - '2016-01-01'::timestamp )
AS new_date;
```

![image](https://github.com/user-attachments/assets/83437b59-a39c-4882-be68-d0d621a71d2b)

```sql
SELECT ( current_timestamp + '1 mon'::interval ) AS new_date;
```

![image](https://github.com/user-attachments/assets/adca6c51-e30f-40e6-b1fb-7d0a1d7c0b5b)

Без `AS new_date`

![image](https://github.com/user-attachments/assets/f4e49e9f-babf-4df9-95b1-7202484ff3ae)

## 21 задание

```sql
SELECT ( '2016-01-31'::date + '1 mon'::interval ) AS new_date;
SELECT ( '2016-02-29'::date + '1 mon'::interval ) AS new_date;
```

![image](https://github.com/user-attachments/assets/25551979-c52e-417f-a7e3-6523e16bff20)

![image](https://github.com/user-attachments/assets/08fd6184-457d-4a0f-bddb-6171b9d068e5)

## 22 задание

![image](https://github.com/user-attachments/assets/c7549dcc-3ef3-4cc3-a375-164a2bc03d5c)

## 23 задание

```sql
SELECT ( '2016-09-16'::date - '2015-09-01'::date );
SELECT ( '2016-09-16'::timestamp - '2015-09-01'::timestamp );
```

![image](https://github.com/user-attachments/assets/728ccfaf-494b-43e8-94e6-114552593c10)

![image](https://github.com/user-attachments/assets/ed8aa6d9-d209-4988-93f8-2068472d4a32)

Почему так см. 19 задание

## 24 задание

```sql
SELECT ( '20:34:35'::time - 1 );
SELECT ( '2016-09-16'::date - 1 );
```

![image](https://github.com/user-attachments/assets/f3f59809-ef2b-4996-8aea-d34772f43412)

![image](https://github.com/user-attachments/assets/95ffd1b2-87bf-49bb-b09a-2b98a2b634bc)

Почему так см. 19 задание. Чтобы работал первый запрос, надо привести 1 к `interval` или `time`

## 25 задание

```sql
SELECT date_trunc('microsecond' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('millisecond' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('sec' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('min' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('hour' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('day' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('week' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('month' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('year' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('decade' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('century' from timestamp '1999-11-27 12:34:56.123459');
SELECT date_trunc('millennium' from timestamp '1999-11-27 12:34:56.123459');
```

![image](https://github.com/user-attachments/assets/2df80f10-e7da-470d-8dbe-431d2e1d75e2)

![image](https://github.com/user-attachments/assets/27e1af86-47ff-48e8-a657-8af2ab58832a)

![image](https://github.com/user-attachments/assets/6b5f37d0-f336-4840-91ce-6c7aedd99517)

![image](https://github.com/user-attachments/assets/5eb1b801-172d-429f-8ccc-6234ad35d6f0)

![image](https://github.com/user-attachments/assets/1843edc5-7a67-49be-8c46-66d77b7f4429)

![image](https://github.com/user-attachments/assets/68349009-4c9a-44b8-a880-46b455747af0)

![image](https://github.com/user-attachments/assets/f75a5bae-fd86-4cf1-8c66-22d090e322b8)

![image](https://github.com/user-attachments/assets/ec0f4a20-f998-48c1-933a-df6b95f02bf0)

![image](https://github.com/user-attachments/assets/bd876f4f-0bf7-461c-9f00-9cb4d4cc3f5b)

![image](https://github.com/user-attachments/assets/43d1fc74-2884-4646-9ae8-d44ed5eb8d81)

![image](https://github.com/user-attachments/assets/d38ac191-5837-4e98-ba7a-f5a31f891941)

![image](https://github.com/user-attachments/assets/ffca7f66-d474-439d-90ca-23e1c2fefddb)

## 26 задание

```sql
SELECT date_trunc('sec', current_timestamp - '0001-01-01 00:00:00.00'::timestamp);
```

![image](https://github.com/user-attachments/assets/774f15e9-2870-4a07-9abf-525f3d7f0255)

## 27 задание

```sql
SELECT extract('microsecond' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('millisecond' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('sec' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('min' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('hour' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('day' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('week' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('month' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('year' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('decade' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('century' from timestamp '1999-11-27 12:34:56.123459');
SELECT extract('millennium' from timestamp '1999-11-27 12:34:56.123459');
```

![image](https://github.com/user-attachments/assets/e898d2ff-2d74-44bb-ae8c-6f36d03bbc3a)

![image](https://github.com/user-attachments/assets/ad5edf86-28de-4e59-ac1d-b833fa105ef6)

![image](https://github.com/user-attachments/assets/8783c68e-2ece-4a1f-9425-f59c80e94ee3)

![image](https://github.com/user-attachments/assets/2e6f62ec-20fb-443e-9261-745709cde7f9)

![image](https://github.com/user-attachments/assets/a750531b-bbca-4028-a027-4414148898c3)

![image](https://github.com/user-attachments/assets/7c5e27ac-5189-47b0-8eb0-ddee2c2afd06)

![image](https://github.com/user-attachments/assets/e79b3a86-d355-49f4-80b2-56d133e231e9)

![image](https://github.com/user-attachments/assets/fc3ee2ce-39d5-425e-b7ac-6c12e2eebda9)

![image](https://github.com/user-attachments/assets/56a093d8-54fd-4441-a318-56f881c1e7e9)

![image](https://github.com/user-attachments/assets/987fbb6b-64a8-45e4-ab7e-dd4b50835c9f)

![image](https://github.com/user-attachments/assets/b98330b7-4e8f-4f20-a7da-432fdd35812c)

![image](https://github.com/user-attachments/assets/4c3a143c-22c1-4f82-8e88-6a3614017fdf)

## 28 задание

```sql
SELECT extract('sec' from current_timestamp - '0001-01-01 00:00:00.00');
```

![image](https://github.com/user-attachments/assets/c702ba32-aeb5-4b1d-bbcd-0e2dd39ed990)

## 29 задание

Равнозначными являются

```sql
SELECT * FROM databases WHERE NOT is_open_source;
SELECT * FROM databases WHERE is_open_source <> 'yes';
SELECT * FROM databases WHERE is_open_source <> 't';
SELECT * FROM databases WHERE is_open_source <> '1';
```

А здесь выдаст ошибку, потому что нужен явный каст

```sql
SELECT * FROM databases WHERE is_open_source <> 1;
```

## 30 задание

```sql
CREATE TABLE test_bool
(
    a boolean,
    b text
);
```

Сработают (1, 3, 4, 5, 7, 9, 10, 11)

```sql
INSERT INTO test_bool VALUES ( TRUE, 'yes' );
INSERT INTO test_bool VALUES ( 'yes', true );
INSERT INTO test_bool VALUES ( 'yes', TRUE );
INSERT INTO test_bool VALUES ( '1', 'true' );
INSERT INTO test_bool VALUES ( 't', 'true' );
INSERT INTO test_bool VALUES ( true, true );
INSERT INTO test_bool VALUES ( 1::boolean, 'true' );
INSERT INTO test_bool VALUES ( 111::boolean, 'true' );
```

Не сработают (2, 6, 8)

```sql
INSERT INTO test_bool VALUES ( yes, 'yes' );
INSERT INTO test_bool VALUES ( 1, 'true' );
INSERT INTO test_bool VALUES ( 't', truth );

```

## 31 задание

```sql
CREATE TABLE birthdays
( person text NOT NULL,
  birthday date NOT NULL );

INSERT INTO birthdays VALUES ( 'Ken Thompson', '1955-03-23' );
INSERT INTO birthdays VALUES ( 'Ben Johnson', '1971-03-19' );
INSERT INTO birthdays VALUES ( 'Andy Gibson', '1987-08-12' );

SELECT * FROM birthdays
WHERE extract( 'mon' from birthday ) = 3;
```

![image](https://github.com/user-attachments/assets/ec118dcd-982f-425d-88c2-93e645941a40)

```sql
SELECT *, birthday + '40 years'::interval
FROM birthdays
WHERE birthday + '40 years'::interval < current_timestamp;
```

![image](https://github.com/user-attachments/assets/fe9eac9b-f03f-4374-b885-60a5a73d7c16)

```sql
SELECT *, birthday + '40 years'::interval
FROM birthdays
WHERE birthday + '40 years'::interval < current_date;
```

![image](https://github.com/user-attachments/assets/c0a5b8f6-5e8a-4c3b-a494-d65fed4c40d5)

```sql
SELECT *, ( current_date::timestamp - birthday::timestamp )::interval
FROM birthdays;
```

![image](https://github.com/user-attachments/assets/8e29ef0b-39c9-4859-b503-2e993119b640)

```sql
SELECT *, justify_days( current_date::timestamp - birthday::timestamp )::interval
FROM birthdays;
```

![image](https://github.com/user-attachments/assets/c4b494d0-95d3-41cc-bdb5-9d073ef94fd6)

## 32 задание

```sql
SELECT array_cat( ARRAY[ 1, 2, 3 ], ARRAY[ 3, 5 ] );

SELECT array_remove( ARRAY[ 1, 2, 3 ], 3 );

SELECT ARRAY[1,4,3] @> ARRAY[3,1,3];

SELECT 3 || ARRAY[4,5,6];

SELECT array_positions(ARRAY['A','A','B','A'], 'A');

SELECT array_to_string(ARRAY[1, 2, 3, NULL, 5], ',', '*');

SELECT unnest(ARRAY[['foo','bar'],['baz','quux']]);
```

![image](https://github.com/user-attachments/assets/f6b8dae9-249e-46fc-9639-74e2ea327266)

![image](https://github.com/user-attachments/assets/0802e0e3-5ef9-4236-8434-b2dc0977efcb)

![image](https://github.com/user-attachments/assets/ead6de79-0ef4-4036-9edf-ee8bc62c371d)

![image](https://github.com/user-attachments/assets/4bff477c-40ad-4c2e-8b11-0bf62108ed4c)

![image](https://github.com/user-attachments/assets/814676ff-f769-4d3d-8a28-5b3816dc1333)

![image](https://github.com/user-attachments/assets/1a2aaf16-c7cc-4159-8355-875fcee70984)

![image](https://github.com/user-attachments/assets/f6341006-d2e9-46db-87f3-a8e77d5c4187)

## 33 задание

```sql
CREATE TABLE pilots
( pilot_name text,
schedule integer[],
meal text[]
);

INSERT INTO pilots
VALUES ( 'Ivan', '{ 1, 3, 5, 6, 7 }'::integer[],
'{ "сосиска", "макароны", "кофе" }'::text[]
),
( 'Petr', '{ 1, 2, 5, 7 }'::integer [],
'{ "котлета", "каша", "кофе" }'::text[]
),
( 'Pavel', '{ 2, 5 }'::integer[],
'{ "сосиска", "каша", "кофе" }'::text[]
),
( 'Boris', '{ 3, 5, 6 }'::integer[],
'{ "котлета", "каша", "чай" }'::text[]
);

SELECT * FROM pilots;
```

![image](https://github.com/user-attachments/assets/36727315-c60c-465c-bc25-5196de5cba07)

```sql
SELECT * FROM pilots WHERE meal[ 1 ] = 'сосиска';
```

![image](https://github.com/user-attachments/assets/f1773a23-1fb9-4ba7-a209-d31ed678f2d0)

```sql
CREATE TABLE pilots_moded
( pilot_name text,
schedule integer[],
meal text[][]
);

INSERT INTO pilots_moded
VALUES ( 'Ivan', '{ 1, 3, 5, 6, 7 }'::integer[],
'{{ "сосиска", "макароны", "кофе" },{ "котлета", "каша", "кофе" }}'::text[][]
),
( 'Petr', '{ 1, 2, 5, 7 }'::integer [],
'{{ "котлета", "каша", "кофе"},{"котлета", "каша", "чай" }}'::text[][]
),
( 'Pavel', '{ 2, 5 }'::integer[],
'{ {"сосиска", "каша", "кофе" },{"сосиска", "каша", "кофе" }}'::text[][]
),
( 'Boris', '{ 3, 5, 6 }'::integer[],
'{{ "котлета", "каша", "кофе"},{"котлета", "каша", "чай" }}'::text[][]
);

SELECT * FROM pilots_moded;

SELECT * FROM pilots_moded WHERE meal[2][ 3 ] = 'кофе';
```

![image](https://github.com/user-attachments/assets/9f6f4838-ce1d-4c97-ac1f-168206c41174)

![image](https://github.com/user-attachments/assets/f258356e-79d8-416d-b05b-908c648216ad)

## 34 задание

```sql
CREATE TABLE pilot_hobbies
(
pilot_name text,
hobbies jsonb
);

INSERT INTO pilot_hobbies
VALUES ( 'Ivan',
'{ "sports": [ "футбол", "плавание" ],
"home_lib": true, "trips": 3
}'::jsonb
),
( 'Petr',
'{ "sports": [ "теннис", "плавание" ],
"home_lib": true, "trips": 2
}'::jsonb
),
( 'Pavel',
'{ "sports": [ "плавание" ],
"home_lib": false, "trips": 4
}'::jsonb
),
( 'Boris',
'{ "sports": [ "футбол", "плавание", "теннис" ],
"home_lib": true, "trips": 0
}'::jsonb
);
```

```sql
UPDATE pilot_hobbies
SET hobbies = jsonb_set( hobbies, '{ trips }', '10' )
WHERE pilot_name = 'Pavel';

SELECT pilot_name, hobbies->'trips' AS trips FROM pilot_hobbies;
```

![image](https://github.com/user-attachments/assets/ddf6043b-1895-4dee-8b10-9e906620058e)

```sql
UPDATE pilot_hobbies
SET hobbies = jsonb_set( hobbies, '{ home_lib }', 'false' )
WHERE pilot_name = 'Ivan';

SELECT pilot_name, hobbies->'trips' AS trips FROM pilot_hobbies;
```

![image](https://github.com/user-attachments/assets/699c8548-1ec5-464c-a3ac-ef52f18bfa4f)

## 35 задание

```sql
SELECT '{ "sports": "хоккей" }'::jsonb || '{ "trips": 5 }'::jsonb;
```

![image](https://github.com/user-attachments/assets/b599edae-654b-4b1a-84af-c572698878a6)

## 36 задание

```sql
SELECT * FROM pilot_hobbies;

UPDATE pilot_hobbies
SET hobbies = hobbies || '{ "travel": [ "USA" ] }'
WHERE pilot_name = 'Pavel';
```

![image](https://github.com/user-attachments/assets/dde8ad7a-70c5-4688-a21e-3b74714342b2)

## 37 задание

```sql
UPDATE pilot_hobbies
SET hobbies = hobbies - 'trips'
WHERE pilot_name = 'Boris';

SELECT * FROM pilot_hobbies;
```

![image](https://github.com/user-attachments/assets/4b425c9a-dd9d-426e-95b7-95758c0f8531)
