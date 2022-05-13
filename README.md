# sql-ex решения задач
SELECT

1.
Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd

решение:
SELECT model, speed, hd
FROM PC
WHERE price < 500

2.
Найдите производителей принтеров. Вывести: maker

решение:
SELECT DISTINCT maker
FROM Product
WHERE type = 'Printer'

3.
Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.

решение:
SELECT model, ram, screen
FROM Laptop
WHERE price > 1000

4.
Найдите все записи таблицы Printer для цветных принтеров.

решение:
SELECT *
FROM Printer
WHERE color = 'y'

5.
Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.

решение:
SELECT model, speed, hd
FROM PC
WHERE (cd = '12x' OR cd = '24x') AND price < 600

6.
Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.

решение:
SELECT DISTINCT Product.maker, Laptop.speed
FROM Product join Laptop on Product.model = Laptop.model
WHERE Laptop.hd >= 10

7.
Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).

решение:
SELECT PC.model, PC.price 
FROM PC 
JOIN Product ON PC.model = Product.model
WHERE maker LIKE 'B'
UNION
SELECT Laptop.model, Laptop.price 
FROM Laptop 
JOIN Product ON Laptop.model = Product.model
WHERE maker LIKE 'B'
UNION
SELECT Printer.model, Printer.price 
FROM Printer 
JOIN Product ON Printer.model = Product.model
WHERE maker LIKE 'B'

8.
Найдите производителя, выпускающего ПК, но не ПК-блокноты.

решение:
SELECT maker 
FROM Product
WHERE type = 'PC'
EXCEPT
SELECT maker 
FROM Product
WHERE type = 'Laptop'

9.
Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker

решение:
SELECT Product.maker
FROM Product
JOIN PC ON Product.model = PC.model
WHERE PC.speed >= 450
GROUP BY maker

10.
Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price

решение:
SELECT model, price
FROM Printer
WHERE price = (SELECT MAX(price) FROM Printer)

11.
Найдите среднюю скорость ПК.

решение:
SELECT AVG(speed)
FROM PC

12.
Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.

решение:
SELECT AVG(speed)
FROM Laptop
WHERE price > 1000

13.
Найдите среднюю скорость ПК, выпущенных производителем A.

решение:
SELECT AVG(speed)
FROM Product
JOIN PC ON Product.model = PC.model
WHERE (Product.type = 'PC' AND Product.maker LIKE 'A')

14.
Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.

решение:
SELECT Ships.class, name, country
FROM Ships
JOIN Classes ON Ships.class = Classes.class
WHERE numGuns >= 10

15.
Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD

решение:
SELECT hd
FROM PC
GrOUP BY hd
HAVING COUNT(code) > 1

16.
Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.

решение:
SELECT DISTINCT A.model, B.model, B.speed, A.ram
FROM PC AS A, PC AS B
WHERE A.ram = B.ram AND A.speed = B.speed AND A.model > B.model

17.
Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.
Вывести: type, model, speed

решение:
SELECT DISTINCT type, Laptop.model, Laptop.speed 
FROM Laptop JOIN Product ON Laptop.model = Product.model 
WHERE Laptop.speed < (SELECT MIN(PC.speed) FROM PC)

18.
Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price

решение:
SELECT DISTINCT maker, price
FROM Product JOIN Printer ON Product.model = Printer.model
WHERE price = (SELECT MIN(Printer.price) FROM Printer WHERE color = 'y') AND color = 'y'

19.
Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.
Вывести: maker, средний размер экрана.

решение:
SELECT Product.maker, AVG(Laptop.screen)
FROM Product JOIN Laptop ON Product.model = Laptop.model
GROUP BY Product.maker

20.
Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.

решение:
SELECT maker, COUNT(model)
FROM Product
WHERE type = 'PC'
GROUP BY maker
HAVING COUNT(model) >= 3

21.
Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC.
Вывести: maker, максимальная цена.

решение:
SELECT Product.maker, MAX(PC.price)
FROM Product JOIN PC ON Product.model = PC.model
GROUP BY Product.maker

22.
Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.

решение:
SELECT speed, AVG(price)
FROM PC
WHERE speed > 600
GROUP BY speed

23.
Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker

решение:
SELECT Product.maker
FROM   Product
JOIN   PC ON Product.model = PC.model
WHERE  PC.speed >= 750
iNTERSECT
SELECT Product.maker
FROM   Product
JOIN   Laptop ON Product.model = Laptop.model
WHERE  Laptop.speed >= 750

24.
Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции.

решение:
SELECT model 
FROM (
       SELECT model, price FROM PC
       UNION
       SELECT model, price FROM Laptop
       UNION
       SELECT model, price FROM Printer) AS model
WHERE price IN(
            SELECT MAX(price) FROM ( 
                         SELECT price FROM PC
                         UNION
                         SELECT price FROM Laptop
                         UNION
                         SELECT price FROM Printer) AS model)
                         
25.
Найдите производителей принтеров, которые производят ПК с наименьшим объемом RAM и с самым быстрым процессором среди всех ПК, имеющих наименьший объем RAM. Вывести: Maker

решение:
SELECT DISTINCT Product.maker FROM Product
WHERE type = 'Printer'
INTERSECT
SELECT DISTINCT maker FROM Product JOIN PC ON Product.model = PC.model
WHERE type = 'PC' AND PC.speed IN(SELECT MAX(speed) FROM PC WHERE ram = (SELECT MIN(ram) FROM PC)) AND PC.ram = (SELECT MIN(ram) FROM PC)

26.
Найдите среднюю цену ПК и ПК-блокнотов, выпущенных производителем A (латинская буква). Вывести: одна общая средняя цена.

решение:
SELECT AVG(price) FROM (
SELECT PC.price 
FROM Product JOIN PC ON Product.model = PC.model WHERE maker = 'A'
UNION ALL
SELECT Laptop.price 
FROM Product JOIN Laptop ON Product.model = Laptop.model WHERE maker = 'A')
AS price

27.
Найдите средний размер диска ПК каждого из тех производителей, которые выпускают и принтеры. Вывести: maker, средний размер HD.

решение:
SELECT maker, AVG(hd)
FROM Product JOIN PC ON Product.model = PC.model
WHERE type = 'PC' AND maker IN (SELECT maker FROM Product WHERE type = 'Printer')
GROUP BY maker

28.
Используя таблицу Product, определить количество производителей, выпускающих по одной модели.

решение:
SELECT COUNT(*) 
FROM ( SELECT maker FROM Product GROUP BY maker HAVING COUNT(maker) = 1) As ProductionCount


29.
В предположении, что приход и расход денег на каждом пункте приема фиксируется не чаще одного раза в день [т.е. первичный ключ (пункт, дата)], написать запрос с выходными данными (пункт, дата, приход, расход). Использовать таблицы Income_o и Outcome_o.

решение:
SELECT i.point, i.date, i.inc, o.out
FROM Income_o i
LEFT JOIN Outcome_o o ON i.point = o.point AND i.date = o.date
UNION
SELECT o.point, o.date, i.inc, o.out
FROM Outcome_o o
LEFT JOIN Income_o i ON o.point = i.point AND o.date = i.date

30.
В предположении, что приход и расход денег на каждом пункте приема фиксируется произвольное число раз (первичным ключом в таблицах является столбец code), требуется получить таблицу, в которой каждому пункту за каждую дату выполнения операций будет соответствовать одна строка.
Вывод: point, date, суммарный расход пункта за день (out), суммарный приход пункта за день (inc). Отсутствующие значения считать неопределенными (NULL).

решение:
SELECT point, date, SUM(out), SUM(inc)
FROM(
SELECT point, date, null AS out, SUM(inc) AS inc
FROM Income
GROUP BY point, date
UNION 
SELECT point, date, SUM(out) AS out, NULL AS inc
FROM Outcome
GROUP BY point, date) AS InOut
GROUP BY point, date
