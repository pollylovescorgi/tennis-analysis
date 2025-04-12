# tennis-analysis
Анализ данных о победителях турниров Большого шлема с 1950 по 2023 гг. (в мужской сетке)

Всем привет! Здесь собраны придуманные мной запросы для погружения в теннисную статистику в PostgreSQL 

1. Определить по каждому турниру Большого шлема количество победителей, которые играли в финале с сеяными теннисистами (пояснение: по правилам сеяных в теннисе 32, а сама система посева была введена в 2001 году).

```python
select "TOURNAMENT", COUNT("WINNER") as num_of_winners
from mens_tennis_grand_slam_winner mtgsw 
where "RUNNER-UP_ATP_RANKING" <= 32 and "YEAR" >= 2001
group by "TOURNAMENT"
order by num_of_winners DESC
```

2. Определить количество леворуких и праворуких теннисистов-победителей ТБШ. 

```python
select "WINNER_LEFT_OR_RIGHT_HANDED", count("WINNER") as num_of_winners
from mens_tennis_grand_slam_winner mtgsw 
group by "WINNER_LEFT_OR_RIGHT_HANDED" 
order by num_of_winners DESC
```
3. Вывести топ-10 победителей ТБШ по сумме призовых (пояснение: в таблице есть пропуски, их лучше исключить).

```python
select distinct "WINNER", SUM("WINNER_PRIZE") as sum_prize
from mens_tennis_grand_slam_winner mtgsw
where "WINNER_PRIZE" > 0
group by "WINNER" 
order by sum_prize DESC
limit 10
```

4. Определить годы турниров, в которых в финале играли двое первых по посеву.

```python
select "YEAR"
from mens_tennis_grand_slam_winner mtgsw 
where "WINNER_ATP_RANKING" in (1,2) and "RUNNER-UP_ATP_RANKING" in (1,2)
group by "YEAR" 
```
5. Вывести, сколько представителей РАЗНЫХ национальностей выигрывали каждый ТБШ.

```python
select "TOURNAMENT", count(distinct("WINNER_NATIONALITY")) as num_of_nat
from mens_tennis_grand_slam_winner
group by "TOURNAMENT" 
```