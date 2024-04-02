# Базы данных и SQL (семинары)

![pictures MySQL for SQL](https://github.com/alexkip66/SQL_Lesson_6/blob/main/MySQL.jpg)

## Урок 6. SQL – Транзакции. Временные таблицы, управляющие конструкции, циклы

#### 1. Создайте функцию, которая принимает кол-во сек и далее переводит их в кол-во дней, часов, минут, секунд. 

Пример: 123456 -> '1 days 10 hours 17 minutes 36 seconds'

*Решение*
```
DELIMITER $$
DROP FUNCTION IF EXISTS FormatPeriod;
CREATE FUNCTION FormatPeriod(seconds INT)
	RETURNS VARCHAR(255)
	DETERMINISTIC
BEGIN
    DECLARE days INT;
    DECLARE hours INT;
    DECLARE mins INT;
    DECLARE secs INT;

    SET days = TRUNCATE(seconds / (24 * 3600), 0);
    SET seconds = MOD(seconds, 24 * 3600);
    SET hours = TRUNCATE(seconds / 3600, 0);
    SET seconds = MOD(seconds, 3600);
    SET mins = TRUNCATE(seconds / 60, 0);
    SET secs = MOD(seconds, 60);
   
    RETURN CONCAT(days, ' days ', hours, ' hours ', mins, ' minutes ', secs, ' seconds ');
END$$
DELIMITER ;
```

*Вызов функции*

```
SELECT FormatPeriod(123456);
```


#### 2. Cоздайте процедуру, которая выведет только числа, делящиеся на 15 или 33 в промежутке от 1 до 1000.

Пример: 15,30,33,45...

*Решение*
```
CREATE TEMPORARY TABLE IF NOT EXISTS TempNumbers(
    number INT
);

DELIMITER $$
DROP PROCEDURE IF EXISTS NumbersDivisible;

CREATE PROCEDURE NumbersDivisible()
BEGIN
    DECLARE num INT DEFAULT 1;
        WHILE num <= 1000 DO
        IF MOD(num, 15) = 0 OR MOD(num, 33) = 0 THEN
            INSERT INTO TempNumbers (number) VALUES (num);
        END IF;
        SET num = num + 1;
    END WHILE;
END$$
DELIMITER ;
```

*Вызов процедуры*

```
CALL NumbersDivisible();

SELECT * FROM TempNumbers;
```



*Подготовил студент Geek Brains* [**`Кипятков Александр`**](https://gb.ru/users/ab4c3ccb-5ede-4cfe-ac50-f1f79d74f8f5), SQL_Seminar_6
