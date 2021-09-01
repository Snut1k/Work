# SQL

# 1.Простые запросы
## SELECT (1.1)
Конструкция Select
~~~
Select * From products
~~~
где * - это все столбцы таблицы
## Where (1.2)
Where - оператор для отбора по критерию. 
~~~
Select name, price from products where price >= 10000
~~~
Выбираем только name и price из таблицы, . Доп условие что price < 3000

Можно указать наоборот, чтобы критерий НЕ был равен, для этого нужно использовать !=

Для сравнения тексовых полей необходимо использовать кавычки ''
~~~
Select * From orders Where status != 'cancelled'
~~~
Если имеется множество условий их можно написать через OR,
~~~
Select * From orders 
where status = 'cancelled' OR status = 'returned'
~~~
 но это будет громоздко, поэтому будем использовать оператор in
~~~
Select * From orders where status in( 'cancelled', 'returned')
~~~
результат запроса: все записи где статус равен cancelled и returned 

| id | user_id | products_count | sum   | status    |

+----+---------+----------------+-------+-----------+

| 2  | 18      | 1              | 200   | cancelled |

| 6  | 1       | 2              | 7690  | cancelled |

| 10 | 13      | 7              | 13000 | returned 
## Множественные усовия
## IN
Запрос удаляет все  записи у которых id =3,4,7
~~~
delete from orders 
 where id in (3,4,7);
~~~
## OR (1.3)
Оператор OR соответствует логическому ИЛИ. В данном случае результатом запроса будут все записи подходящие ИЛИ под одно ИЛИ под другое условие
~~~
Select * From orders where sum > 3000 or products_count >= 3
~~~
## AND (1.4)
And соответствует логческому И, результатом запроса будут записи удовлетворяющие сразу двум запросам 
~~~
Select * From orders where sum >= 3000 AND products_count < 3
~~~
## Between (1.5)
Between это оператор для выборки в определенном диапозоне чисел.

При этом он отсеивает сами границы , тоесть записи где sum=3000 и sum=10000, не попадут в сисок.
~~~
Select * From orders where sum between 3000 and 10000 AND status = 'cancelled'
~~~
## NOT (1.6)
NOT инвертирует любой оператор
~~~
Select * from orders where not sum between 3000 and 10000 and status ="cancelled" 
~~~
В данном случае результатом будут все аписи НЕ входящие в промежуток от 3 до 10 тысяч.
И удовлеворяющие условию status ="cancelled" 

| id | user_id | products_count | sum   | status    |

+----+---------+----------------+-------+-----------+

| 3  | 45      | 3              | 800   | cancelled |

| 11 | 13      | 7              | 13000 | cancelled |

## Порядок OR и AND (1.7)
У AND приоритет выполнения перед OR.
Тоесть сначала выпоняется AND а потом OR.
~~~
Select * From team 
Where basic_languadge='python'  OR
basic languadge = 'php'  AND level ='middle' 
~~~
Логика верхнего запроса такова: нужно отправить таблицу с разработчиками php, у которых middle уровень знаний ИЛИ всех разработчиков на python
~~~
Select * From team 
Where (basic_languadge='python'  OR
basic languadge = 'php')  AND level ='middle' 
~~~
Логика нижнего запроса: ищем всех разработчиков на питоне или php И тех из них кто имеет средний уровень разработки.
Как видно из второго запроса, тут  как в математике, очередность меняется если поставить скобк, что в скобках то  выполняется первым.
# Сортировка
## Order by (1.8)
 Это сортировка запросов по одноу из полей от меньшего к большему. С начала и до конца алфавита от более ранней даты к более поздней от самой маленькой зарплаты к самой большой и т.д
 ~~~
 Select * From products order by price
 ~~~
Результат:
| id | name              | count | price |

+----+-------------------+-------+-------+

| 5  | Вентилятор        | 0     | 700   |

| 7  | Тостер            | 2     | 2500  |

| 8  | Принтер           | 4     | 3000  |

| 3  | Микроволновка     | 3     | 4000  |

| 4  | Пылесос           | 2     | 4500  |

| 1  | Стиральная машина | 5     | 10000 |

| 2  | Холодильник       | 0     | 10000 |

| 6  | Телевизор         | 7     | 31740 |
  Если добавить Desc, то сортировка произойдет в обртном направлени
~~~
Select name, price from products order by price desc
~~~
Результат:
 name              | price |

+-------------------+-------+

| Телевизор         | 31740 |

| Стиральная машина | 10000 |

| Холодильник       | 10000 |

| Пылесос           | 4500  |

| Микроволновка     | 4000  |

| Принтер           | 3000  |

| Тостер            | 2500  |

| Вентилятор        | 700   |

Также можно сортировать сразу по нескольким полям перечисляя их через запятую. Сначала база будет сортироваться по первому полю которое мы вписали, потом по второму. 

Мы можем также инвертировать отдельное поле при сортировке дописав к нему desc.

~~~
Select * From users 
where salary >= 40000 
order by salary desc, first_name
~~~
| id | first_name | last_name | birthday   | salary | job                 |

+----+------------+-----------+------------+--------
+---------------------+

| 7  | Александр  | Пузаков   | 2002-02-20 | 120000 | ведущий программист |

| 5  | Алена      | Шикова    | 1999-08-17 | 53000  | фотограф            |

| 2  | Ольга      | Антонова  | 1999-12-01 | 41000  | дизайнер            |

| 8  | Алина      | Антонова  | 2002-01-01 | 40000  | верстальщик         |

| 3  | Сергей     | Васильев  | 2002-02-20 | 40000  | младший программист |

# Ограничение выборки
## limit (1.9)
если limit 5 ; то показываются только первые 5 записей, если limit 10, 5; то НЕ показываются первые 10 и показываются следующие после этих 10 ти 5
~~~
 Select * from products order by price  limit 10,5
~~~

# 2. Добавление, изменение, удаление, данных
создание - insert;

чтение - select;

удаление - Deleate или Truncate ;

обновление - update;
# Добваление
## insert into (2.1) 
## Values
isert into- добавляет строки таблицы;

orders - имя таблицы в которую вставляют данные  ;

id , products , sum -поля таблицы
~~~
insert into orders (id, products, sum)
values (6,3, 3000)
~~~
'(id, products, sum)'- эта часть отвечает за состав  порядок вставки в таблицу.

Если мы напишем (id, sum) и вставим (7,10000), то поле sum у этой записи будет пустым.

Числа не нужно добавлять в кавычки а текст и даты нужно.
Формат записи даты может быть другим в завсимости от используемого.
~~~
insert into products (id, name, count, price)
values
(8,'iPhone 7', 1,59990),
(9,'iPhone 8', 3, 64990),
(10,'iPhone X', 2, 79900);
~~~
(id, name, count, price) Эту часть тоже можно не писать.

## set (2.2) 
set имеет следующую конструкцию

INSERT INTO table SET field1=value1, field2=value2;
~~~
insert into users 
set id=10,first_name='Никита', last_name= 'Петров';
~~~

## update (2.3) 
update -  преднаначен для изменения полей таблицы, products это имя таблицы, set задает полю name занчение iMac, в той аписи где id = 7
~~~
Update products set name = 'iMac' where id =7;
~~~
## set (2.4) 
set - это как и какие мы изменяем поля мы изменяем поля:
~~~
Update users set 
salary=salary*1.1 where salary < 20000
~~~


## NULL (2.5) 
NULL – это особое слово в MySQL и в отличии от "cancelled" или "new", его нужно писать без кавычек.
А чтобы сравнить значение в поле с NULL, нужно использовать не символы равенства (=) и неравенства (<> или !=),
а специальное выражение IS NULL или IS NOT NULL.
~~~
update orders set status='new' 
where status is NULL
~~~
Здесь такая же ситуация для where как и в select, можно стоавить несколько условий через  OR или AND
~~~
update orders set amount=sum*products_count 
where amount =0 or amount is null
~~~
~~~
update products set count = count + 40 where name= 'Марс' or name = 'Сникерс' 
~~~
Можно задавать поля чтобы они не были равны NULL
~~~
create table cars(
 id int NOT NULL,
 mark Varchar(20) NOT NULL,
 model Varchar(20) NOT NULL,
 year year, 
 mileage mediumint unsigned
);
~~~
При этом если мы вставим в такое значение '', то будет показываться null, но это будет пустая строка. NULL это ничто,пустая ячейка данных. Допустим мы хотим найти всех немужчин в таблице(варианты: m, w, пустая строка, и NULL )
~~~
select * From users 
 where sex <> m; 
~~~
Нам выдадаст всех женжин и людей у которых пустая строка вместо пола.Если нужно найти и Null в том числе запрос будет следующий
~~~
select * From users 
 where sex <> m or sex is NULL
~~~

## limit (2.6) 
Тут все также как и в select, можно задать направление исполнения комманды с помощбю desc, и задать параметры, а также установить лимит
~~~
update products set price = price*1.05 
order by price desc limit 5
~~~

## (2.7) Delete
Удаляет толбцы из таблицы. К delete применимы те же принципы что и к select, update, insert 
~~~
delete from cars 
where country = 'JP' and (power >129 or power < 79)
~~~

# 3.Создание таблиц
boolean в mysql НЕ существует, при попытке его добавить, добавляется tinyint c значениями 0 или 1 
## Create (3.1)
Создает таблицу (в даном случае messages )где id и т.д это поля.

ПОСЛЕ ПОСЛЕДНЕГО ПОЛЯ НЕ НУЖНА ЗАПЯТАЯ!

(inset into добавляет данные в таблицу)
~~~
Create table messages (
 id int,
 subject Varchar(100),
 message text,
 add_date Datetime,
 is_public bool
 );
insert into messages (id, subject, message, add_date, is_public)
 values 
 (1, 'Первое сообщение','Это мое первое сообщение!', '2016-12-12 14:16:00', true);
~~~
~~~
create table rating (
 id int,
 car_id int,
 user_id int,
 rating float
);
insert into rating ( id,car_id, user_id, rating)
 values
 (1,1,1,4.54),
 (2,1,2,3.34),
 (3,2,3,4.19),
 (4,2,11,1.12);
~~~

## unsigned (3.3) 
В int храняться числа от -2147483648  до + 2147483647.

Там где мы не используем минусоые значения используем unsigned, чтобы исключить хранение минусовых значений

~~~
id int unsigned;
age tinyint unsigned 
~~~
Теперь диапозон int будет равен от 0 до 4294967296,а tinyint от 0 до 255
~~~
create table films (
    id INT unsigned,
    name varchar (100),
    rating float unsigned,
    country varchar (2)
);
insert into films (id, name, rating, country)
 Values
 (1,'Большая буря', 3.45,'RU'),
 (2,'Игра', 7.5714,'US'),
 (3,'Война', 10.0,'RU');
~~~
## Decimal (3.4)
~~~
amount Decimal (10,2)
~~~
Где 10 максимальное число знаков в числе, 
а 2 после запятой. 

Максиальное число в данном случае - 8мь девяток до запятой и две после запятой: 99999999.99

Если вставлять числа после запятой, то происходит округление

## Строковые поля
~~~
'\'Хорошее'\' кино\n про упорстово' =

'Хорошее' кино

про упорство
 ~~~
## Varchar  (3.5)  
 Varchar это строковый тип данных в ктором максимальное значение символов 65535 символов.
Лучше не ставить индекс у varchar больше чем нужно. Например не будет имя человека больше 20 ти символов, а фамилии больше 50. 

## Text (3.6) 
Text - фиксировынный текстовой тип писать скобки не нужно, выдаст ошибку.
~~~
Create table users(
 id INT unsigned,
 first_name Varchar (50),
 last_name Varchar (60),
 bio text 
);

insert into users (id, first_name, last_name, bio)
 Value
 (1,'Антон','Кулик','С отличием окончил 39 лицей.'),
 (2,'Сергей','Давыдов',''),
 (3,'Дмитрий', 'Соколов','Профессиональный программист.');
 Select * From users
~~~
Text - 65535
Mediumtext - 16777215
Longtext - 4294967295 Максимальные количества знаков
## Blob (3.7)
Создан для хранения двоичных объектов (изображений, звуков, электронных документов).
Но лучше не хранить их в бд, а хранить ссылкой на жесткий диск.
## Дата и время
## Datetime (3.8)
 записывается в формате
 ~~~
'2017-04-04 05:12:07' или
'2014-12-13'
 ~~~ 
 Пример:
 ~~~
 reate table users (
 id int unsigned,
 email Varchar (100),
 date_joined date,
 last_activity datetime
);
insert into users
 value
 (1,'user1@domain.com', '2014-12-12', '2016-04-08 12:34:54'),
 (2,'user2@domain.com', '2014-12-12', '2017-02-13 11:46:53'),
 (3,'user3@domain.com', '2014-12-13', '2017-04-04 05:12:07');
 ~~~
 Для хранения микро и милисекунд, нужно чтобы  при содании таблцы у поля в котором хранится врямя был параметр
 ~~~
 Create table
 int tinyint unsigned,
 payment_date DATETIME (3)
 ~~~
 Где 3- количество знаков после запятой у секунд. (3)- для милисекунд, (6) для микросекунд.
 Datetime  округляет время, если имеются лишние цифры. При последующем увелицении параметра или при внесении только даты без времени все незаполненные знаки ставятся как 0. 
##  (3.9) NOT NULL
Можно задавать поля чтобы они не были или наоборот были равны NULL
~~~
Create table products(
 id int unsigned not null,
 name Varchar (120) not null,
 category_id int unsigned  null ,
 price Decimal(10,2) not null
); 
insert into products (id,name, category_id, price)
Value
(1, 'Подгузники (12 шт)', 3, 700),
(2, 'Подгузники (24 шт)', 3, 1250),
(3, 'Спиннер', NULL, 250.40),
(4, 'Пюре слива',4, 47.50);
Select * From products
~~~
При этом если мы вставим в такое значение '', то будет показываться null, но это будет пустая строка. NULL это ничто,пустая ячейка данных. Допустим мы хотим найти всех немужчин в таблице(варианты: m, w, пустая строка, и NULL )
~~~
select * From users 
 where sex <> m; 
~~~
Нам выдадаст всех женжин и людей у которых пустая строка вместо пола.Если нужно найти и Null в том числе запрос будет следующий
~~~
select * From users 
 where sex <> m or sex is NULL
~~~
##  BOOL (3.10)
boolean в mysql НЕ существует, при попытке его добавить, добавляется tinyint c значениями 0 или 1.
Операции по выборке из таблицы работают в случае с TRUE, FALSE, И с 0,1.
## Enum (3.11)
Хранит только заранее определенные значения. Он не может хранить сразу draft и correction.
~~~
Create table articles(
 id int unsigned not null,
 name Varchar(80),
 text text null,
 state enum('draft','correction','public')
);
Insert into articles
 Value
(1,'Новое в Python 3.6','','draft'),
(2,'Оптимизация SQL запросов','При больших объемах данных ...','correction'),
(3,'Транзакции в MySQL','По долгу службы мне приходится ...','public');
~~~
## Set (3.12)
Он может хранить сразу и bar и conditioner и fridge и wifi через запятую
~~~
additional set('conditioner','bar','fridge','wifi')
~~~
выглядит это так - conditioner,bar,fridge,wifi
~~~
(456,763,14299, 'pack,call')
~~~
так выглядит вставка записи, последнее это SET, pack и call идут через запятую без символов между ними
## find in set (3.13)
Ищет все записи где есть RU и BY тоесть он найдет записи: (RU) (RU,BY) (KZ,RU), (KZ,BY) и т.д.

Если искать через country = 'RU', То найдет только RU, если искать country = 'RU,BY' то не найдет ничего. Тк запятая это чисто разделительный знак
~~~
Select name, price ,country from products
 where (find_in_set ('RU',country) or find_in_set('BY',country)) and category_id is not null
 order by price desc
~~~
## Default(3.14)
Default задает значения по умолчанию, они могут быть логическими, числовыми, текствыми и т.д
~~~
Create table orders(
    id int unsigned not null,
    user_id int unsigned not null,
    amount mediumint unsigned not null default 0,
    created datetime not null,
    state enum('new','cancelled','in_progress','delivered','completed') not null default 'new'
);
~~~
Пример
~~~
insert into orders
 values
(3,78,2200,'2018-02-01 22:43:09',default);
~~~
## current_timestamp (3.15)
Нужен для запоминания настоящей даты и времени. Если применять к datetime, то без мили и микросекунд, они просо не запишутся.
Во второй записи date , будет дата и время запроса.
~~~
create table reviews(
    id int unsigned not null,
    user_id int unsigned not null,
    date datetime not null default current_timestamp
);
insert into reviews
 values
 (1,817,'2018-01-11 19:50:01'),
 (2,1289,default);
~~~
# 4.Индексы
## Первичный ключ
##  primary key (4.1)
Присваевает одному или нескольким полям статус первичного ключа, который позволяет однозначно различать запси в таблице тк оно уникально 
~~~
Create table files(
    id int unsigned not null primary key auto_increment,
    film_id int unsigned not null,
    size bigint unsigned,
    filename Varchar(100)
);
~~~
~~~
    primary key (series,number)
~~~

##  auto_increment (4.2)
Автоматически выставяет номера по порядку 1,2,3 и тд. В данном случае выставляется поле id
~~~
create table orders(
    id int unsigned not NULL primary key auto_increment,
    state Varchar(8),
    amount decimal (8,2) 
);
~~~
## Уникальный ключ
## unique key (4.3)
Создет уникальный ключ для поля(то что в скобках)
~~~
unique key passport (passport)
~~~
Если нужно чтобы конкретная комбнация серии и номера не повторялась:
~~~
unique key passport (series, number)
~~~
## Обычные индексы
##  index (4.4)
Если много записей в таблице, то  поиск информации сановится длительным по времени и возрастает нагрузка на жесткй диск, оперативную память, и т.д

Индексы хранят информацию о том где хранятся данные, выстраивает(сортирует) данные по порядку. Похож на картотеку где все книги расположенны по алфавиту.

Запрос создает индекс main search в который входят city_id, и state при создании таблицы
~~~
index main_search (city_id,state)
~~~
Создание индекса обычным запросом. base_query- имя индекса, calendar- таблица в которой содержатся поля  city_id,date, rкоторые собственно и индексируются.
~~~
create index base_query on calendar(city_id,date)
~~~
Удалять индексы можно тоько по одному
~~~
Drop index number on passports ;
Drop index series on passports ;
~~~
##  (4.5)
##  (4.6)
##  (4.7)
##  (4.8)
##  (4.8)
##  (4.10)
##  (4.11)
##  (4.12)
##  (4.13)

## определение остатка
Данный знак (%) находит статок при изменении или ыборке из таблицы
~~~
1 % 2 = 1 (остаток 1, нечетное)
2 % 2 = 0 (остаток 0, четное)
3 % 2 = 1 (остаток 1, нечетное)

Select id, name from products 
 where id%2=0 order by price
~~~
Запрос выдаст записи со всеми id которые делятся на 2 без остатка.
## Способы посмотреть какие поля у таблицы
products- имя таблицы.
~~~
Describe products;
~~~
~~~
Show Fields from products ;
~~~
Первые два запроса идентичны :
~~~
| Field | Type         | Null | Key | Default | Extra |
+-------+--------------+------+-----+---------+-------+
| id    | int unsigned | NO   | PRI | NULL    |       |
| name  | varchar(255) | YES  |     | NULL    |       |
| count | int          | YES  |     | NULL    |       |
| price | int          | YES  |     | NULL    |       |
+-------+--------------+------+-----+---------+-------+
~~~

~~~
SHOW CREATE TABLE products;
~~~
Выдает следующее:
~~~
+----------+--------------------------------------------------------------------+
| Table    | Create Table                                                       |
+----------+--------------------------------------------------------------------+
| products | CREATE TABLE `products` (                                          |
|          |   `id` int unsigned NOT NULL,                                      |
|          |   `name` varchar(255) DEFAULT NULL,                                |
|          |   `count` int DEFAULT NULL,                                        |
|          |   `price` int DEFAULT NULL,                                        |
|          |   PRIMARY KEY (`id`)                                               |
|          | ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci |
+----------+--------------------------------------------------------------------+
~~~

## Униальные индексы ???

## Форматы хранения времени
 ## Float 

## constraint
## Инексы и индексирование

## Order by и group  by
## set, update, values, в чем отличия?

## Truncate и Delete, различия
Команда DELETE удаляет записи из таблицы, которые удовлетворяют критерию WHERE.

TRUNCATE удаляет все данные из таблицы.

TRUNCATE

TRUNCATE быстрее и использует меньше системных ресурсов, чем DELETE и практически не пишет лог транзакции.

TRUNCATE удаляет данные путем деаллокации тех страниц, которые хранят табличные данные и только эти операции деаллокации записываются в лог транзакции.

TRUNCATE удаляет все строки таблицы, но структура таблицы (столбцы, ограничения, индексы и т.д.) остается. Счетчик, который используется для уникальности новых записей обнуляется.

Нельзя использовать TRUNCATE TABLE для таблиц, связанных ограничением FOREIGN KEY.
Поскольку TRUNCATE TABLE не логируется, то и не может активировать триггер.

Откат (rollback) после TRUNCATE невозможен

TRUNCATE - команда DDL

DELETE

DELETE удаляет строки в таблицы и для каждой оставляет запись в логе транзакции.

DELETE не обнуляет счетчик уникальности.

DELETE может использоваться с выражением WHERE или без него

DELETE активирует триггеры

После DELETE возможен откат

DELETE - команда DML

