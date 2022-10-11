
## Вопросы на собеседовании в компании 1 (sql)

1. Пусть есть таблица ```Salary```:

```sql
Salary (table)
rowid		first_name		second_name		salary

 1			peter			pan			100
 2			big			bunny			100
 3			ben			johnsen			120
 4			marilyn			monroe			150
 5			donald			trump			150
 6			melania			trump			160
```

a) Вывести все значения зарплат (salary)

Ответ:

```sql
select salary
from Salary
```

b) Вывести все значения зарплат (salary) в порядке возрастания

Ответ:

```sql
select salary
from Salary
order by salary
```

c) Вывести все уникальные значения зарплат (salary)

Ответ:

```sql
select distinct salary
from Salary
```

d) Вывести значения зарплат (salary), которые встречаются больше
одного раза (то есть для примера нужно вывести 100 и 150)

Ответ: использовать ```group by```

2. Какие бывают виды join'ов (left, right, inner, full), рассказать про отличия
3. Есть таблица ```Customers``` и ```Devices```:

```sql
=== Table ( Customers ) ===
rowid	first_name	second_name		age

1 		peter		pan			10
2		big 		bunny 			10
3		ben 		johnsen 		12
4		marilyn 	monroe 			12
5		donald 		trump			15
6		melania		trump 			16

=== Table ( Devices ) ===
id_customer		  name

2 			Camera
3 			Computer
3 			Monitor
4 			Printer
```

Что выведет ```left join``` для этих таблиц?

Ответ: 

```sql
select first_name, name
from Customers c
left join Devices d on c.rowid = d.id_customer
```

Вывод: 

```sql
1, null
2, Camera
3, Computer
3, Monitor
4, Printer
5, null
6, null
```
