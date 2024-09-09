# Задания к главе 3

## 1 задание

Пытаемся выполнить запрос

```sql
INSERT INTO aircrafts
VALUES ( 'SU9', 'Sukhoi SuperJet-100', 3000 );
```

Появляется ошибка, так как в таблице уже есть запись с самолетом под номером `SU9`, а поскольку `aircraft_code` является `PRIMARY_KEY`, то он должен быть уникальным для каждой строки.

## 2 задание

```sql
SELECT * FROM aircrafts
ORDER BY range DESC;
```

## 3 задание

Проверяем изначальную дальность

```sql
SELECT * FROM aircrafts 
WHERE model = 'Sukhou SuperJet-100'
```

Увеличиваем модели `Sukhou SuperJet-100` дальность в 2 раза

```sql
UPDATE aircrafts
SET range = range * 2
WHERE model = 'Sukhou SuperJet-100'
```

Проверяем получившуюся дальность

```sql
SELECT * FROM aircrafts
WHERE model = 'Sukhou SuperJet-100'
```

## 4 задание

```sql
DELETE FROM aircrafts
WHERE range < 0
```
