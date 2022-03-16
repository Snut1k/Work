# SQL
Select
# 1. SELECT basics
## 1.1 Пример выборки из таблицы
Сначала пишем SELECT затем критерий (параметр).

То что мы хотим выбрать для показа идет после SELECT через запятую а имена через IN ('XXX', 'YYY');
~~~
SELECT name, population 
~~~

Если хотим отсортировать числовое значение в определенных рамках то ставим BETWEEN XXX and YYY
~~~
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
~~~
Сначала пишем SELECT затем критерий (параметр) обычно X таблицы в данном случае это население >> FROM >> world-таблица а 'France' имя объекта.
## 1.2 Пример выбора двух параметров (населения и имени)
  ~~~
  SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark') AND population >10000000 ;
  ~~~
  То что мы хотим выбрать для показа идет после SELECT через запятую а имена через IN ('XXX', 'YYY');
  Это необходимо когда у нас много условий применимых к одному столбцу в таблице)
## 1.3 Пример сортировки выбранных значений
~~~
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
~~~
Если хотим отсортировать числовое значение в определенных рамках то ставим BETWEEN XXX and YYY
# 2. SELECT names
'%'-любой набор симвоов(даже пустой)

Чтобы выбрать имя начнающиеся с любого набора символов испоьзуем конструкцию: name Like 'XXX%'

Конструкцию '%y' где %- любой набор символов(в том числе и пустой) выдаст все результаты с y на конце

"_" - это пропущенный символ

"ORDER BY name" - это сотировка в алфавитом порядке по имени можно например по population будет отсортировано от меньшего к большему населению

'=' - это значит что мы ищем ровно такой результат (capital=name+'city')

LIKE - это конструкция для выбора похожих значений
~~~
name LIKE 'af%'
~~~
## 2.1 Находим страну начинающуюся с Y
Чтобы выбрать имя начнающиеся с любого набора символов испоьзуем конструкцию: name Like 'XXX%'
~~~
SELECT name FROM world
  WHERE name LIKE 'Y%'
~~~
## 2.2 Находим страну заканчивающуюся на y
Чтобы это осуществить ставим конструкцию '%y' где %- любой набор символов(в том числе и пустой)
~~~
SELECT name FROM world
  WHERE name LIKE '%y'
~~~
## 2.3 Находим любой набор символов 
~~~
SELECT name FROM world
  WHERE name LIKE '%x%'
~~~
Вместо x можно вставить любой набор символов, если они присутствуют то отобразятся все варианты подходящие под описание
## 2.4 Все страны с окончанием на land
~~~
SELECT name FROM world
  WHERE name LIKE '%land'
~~~
## 2.5 Страны начинающиеся на "С" и заканчивающиеся на "ia"
~~~
SELECT name FROM world
  WHERE name LIKE 'C%ia'
~~~
## 2.6 Cтраны где есть "oo" в названии
~~~
SELECT name FROM world
  WHERE name LIKE '%oo%'
~~~
## 2.7 Страны где есть три буквы а
~~~
SELECT name FROM world
  WHERE name LIKE '%a%a%a%'
~~~
## 2.8 Выбираем страну с второй буквой t
"_" - это пропущенный символ
"ORDER BY name" - это сотировка в алфавитом порядке по имени можно например по population будет отсортировано от меньшего к большему населению
~~~
SELECT name FROM world
 WHERE name LIKE '_t%'
ORDER BY name
~~~
## 2.9 Выбираем страну с двумя буквами o и двумя символами между ними
~~~
SELECT name FROM world
 WHERE name LIKE '%o__o%'
~~~
## 2.10 Выбираем страну с 4 симвлами (4 нижних подчеркивания)
~~~
SELECT name FROM world
 WHERE name LIKE '____'
 ~~~
## 2.11 Выбираем страну с столицей одинаковой имени страны (Люксембург-столица люксембург)
SELECT name
  FROM world
 WHERE name=capital
## 2.12 Выбираем  страну где столица = имя страны + "city"
~~~
SELECT name
  FROM world
 WHERE capital LIKE name +' '+'city'
 ~~~
## 2.13 Выбираем страну где солица содержит имя страны
~~~
SELECT capital, name
  From world
 Where capital Like '%'+name+'%'
 ~~~
## 2.14 Выбираем страну где столица содержит имя страны и длинна столицы больше длинны имени страны
~~~
Select capital, name
  From world
 Where capital Like '%'+name+'%' and capital>name
~~~
## 2.15 Отделяем имя страны от имени столицы и представляем их рядом
~~~
SELECT name, REPLACE(capital, name, '') as extention
FROM world
WHERE capital LIKE concat(name, '_%')
~~~
# 4 SELECT within SELECT
# 4.1 Каждая стана с наслеением большим чем у России
~~~
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
~~~
Сначала выставляем условие для населения, оно больше.
Затем пишем селект внутри и просто выбираем ту страну с которой будем сравнивать.
# 4.2 Страны с ВВП на душу населения в европе большим чем у Великобритании
Можно делить имена при селекте
~~~
SELECT name,GDP/population
From world
WHERE name='United Kingdom'
~~~
После того как нашли ввп на дуду населения у великобритании находим страны в европе с большим показателем
~~~
SELECT name
 FROM world
     WHERE continent='Europe' AND GDP/population>(
SELECT GDP/population
From world
WHERE name ='United Kingdom'
)
~~~
# 4.3 Все страны в алфавитном порядке оттносящиеся к кон
~~~
SELECT name, continent
 FROM world 
   WHERE continent IN ( 
SELECT continent 
  FROM world
    WHERE name IN ('Argentina', 'Australia')
) ORDER BY name
~~~
# 4.4 У каких стран население между польшей и канадой?
~~~
SELECT name, population
FROM world
 WHERE population>(
  SELECT population
   FROM world 
    WHERE name = 'Canada')
AND population<(
  SELECT population
   FROM world 
    WHERE name = 'Poland')
~~~ 
# 4.5
~~~SELECT name, 
  Concat(Round(100*population/(SELECT population 
  FROM world 
    WHERE name = 'Germany'),0),'%') as percentage
    From world
    WHERE continent = 'EUROPE'
~~~
# 4.6 Все страны ВВП которых больше чем ВВП самой большой страны в европе 
~~~
SELECT name
 FROM world
 WHERE GDP> ALL(Select GDP 
  From world 
  Where continent = 'Europe' AND GDP>0)
~~~
# 4.7
~~~
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
~~~
# 4.8 Первые страны по алфавиту на каждом
~~~
Select continent, name
   From World x
    Where name <= ALL(Select name 
    From World y
      Where x.continent=y.continent)
~~~
# 4.9 Континенты со всеми странами с населением меньше 25 млн чел
~~~SELECT name, continent, population 
 FROM world
 WHERE continent IN (SELECT continent 
  FROM world  x 
  WHERE 25000000 >= (SELECT MAX(population) 
    FROM world y 
     WHERE x.continent = y.continent))
~~~
# 4.10 Страны имеющие население в три раза больше любого своего соседа по континенту
~~~
SELECT name, continent FROM world x
  WHERE population > ALL(SELECT 3*population
  FROM world y 
   WHERE x.continent = y.continent
   AND x.name <> y.name)
~~~
(Select * - выбор всей таблицы)
( XXX<>YYY - XXX не равно YYY)
(common table exchange)
(leftjoin двух таблиц)
(еще 6.)
