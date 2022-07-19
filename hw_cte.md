# create table and insert data

CREATE TABLE statistic( 
player_name VARCHAR(100) NOT NULL, 
player_id INT NOT NULL, 
year_game SMALLINT NOT NULL CHECK (year_game > 0), 
points DECIMAL(12,2) CHECK (points >= 0), PRIMARY KEY (player_name,year_game) );
    
    
INSERT INTO statistic(player_name, player_id, year_game, points) 
VALUES ('Mike',1,2018,18), 
('Jack',2,2018,14), ('Jackie',3,2018,30), 
('Jet',4,2018,30), ('Luke',1,2019,16), 
('Mike',2,2019,14), ('Jack',3,2019,15), ('Jackie',4,2019,28), 
('Jet',5,2019,25), ('Luke',1,2020,19), 
('Mike',2,2020,17), ('Jack',3,2020,18), 
('Jackie',4,2020,29), ('Jet',5,2020,27);

# sum points by year (different variants)
# 1:
select SUM(points),year_game from  statistic
GROUP BY year_game
order by year_game;
# 2: 
WITH cte_sum_points (sum_points,year_game ) AS (
select SUM(points),year_game from  statistic
GROUP BY year_game)

select * from cte_sum_points order by year_game;
# 3:
SELECT DISTINCT SUM(points) OVER w,year_game
FROM  statistic
WINDOW w AS (PARTITION BY year_game ORDER BY year_game DESC)
order by year_game;

# lag and lead  
SELECT player_name, player_id, year_game,points,
       lag(points) OVER (PARTITION BY player_name ORDER BY year_game DESC), lead(points) OVER (PARTITION BY player_name ORDER BY year_game DESC)
FROM statistic
ORDER BY player_name,year_game;
 
