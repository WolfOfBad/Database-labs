![image](https://github.com/user-attachments/assets/737aaff8-081a-404e-8a31-7a3868208492)![image](https://github.com/user-attachments/assets/9066cc1f-fb6f-4024-8a64-d00e0201cfd5)# Задания к главе 4

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
SELECT '-Inf'::double precision > 1E-307;
```

![image](https://github.com/user-attachments/assets/c018e6f8-0852-4880-a922-49afb46108a4)

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
