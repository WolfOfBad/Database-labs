# Задания к главе 3

Выполняем запросы из главы (`querries.sql`), чтобы получить таблицы с записями

## 1 задание

Пытаемся выполнить запрос

```sql
INSERT INTO aircrafts
VALUES ( 'SU9', 'Sukhoi SuperJet-100', 3000 );
```

И получаем ошибку

![изображение](https://github.com/user-attachments/assets/4cfde9a8-0a5e-434b-816c-c5d850231c6f)

Эта ошибка появляется, так как в таблице уже есть запись с самолетом под номером `SU9`, а поскольку `aircraft_code` является `PRIMARY_KEY`, то он должен быть уникальным для каждой строки

## 2 задание

```sql
SELECT * FROM aircrafts
ORDER BY range DESC;
```

![изображение](https://github.com/user-attachments/assets/21567ce5-b7dd-41b8-b4b6-b1cd76d5e5cf)

## 3 задание

Проверяем изначальную дальность

```sql
SELECT * FROM aircrafts 
WHERE model = 'Sukhou SuperJet-100'
```

![изображение](https://github.com/user-attachments/assets/5997c17b-1559-43df-8abb-03ed5aebb578)

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

![изображение](https://github.com/user-attachments/assets/9bb7008f-59e2-4919-91ff-997a5c63e140)

## 4 задание

```sql
DELETE FROM aircrafts
WHERE range < 0
```

Никакой ошибки не выкинуло

![изображение](https://github.com/user-attachments/assets/371d9e7e-7b2d-4a76-ade0-f11c23e5a557)

