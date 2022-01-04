### Exercise 4 Of SQL Queries

## Task 1 - 1.

```
CREATE TABLE Coach (
id  INT NOT NULL,
name  TINYTEXT,
mail  TINYTEXT,
phone  TINYTEXT,
from_date  DATE,
hourly_rate  SMALLINT,
institude TINYTEXT,
PRIMARY KEY(id)
#)

CREATE TABLE Types (
type_name  varchar(255) NOT NULL,
description  MEDIUMTEXT NOT NULL,
PRIMARY KEY (type_name)
)

CREATE TABLE Coaches (
coach_id  INT,
type_name varchar(255),
FOREIGN KEY (coach_id) REFERENCES Coach(id),
FOREIGN KEY (type_name) REFERENCES Types(type_name)
)

CREATE TABLE Clients (
c_id  INT,
name  TINYTEXT,
address  TINYTEXT,
phone  TINYTEXT,
PRIMARY KEY (c_id)
)

CREATE TABLE Training_sequence (
c_id  INT NOT NULL,
start_date  DATE NOT NULL,
coach_id  INT,
type_name  varchar(255),
hours  SMALLINT,
FOREIGN KEY (c_id) REFERENCES Clients(c_id),
FOREIGN KEY (coach_id) REFERENCES Coaches(coach_id),
FOREIGN KEY (type_name) REFERENCES Coaches(type_name),
PRIMARY KEY (start_date)
)
```

## Task 1 - 2.

```
INSERT INTO Coach
VALUES
(1, "name1", "somemail@gmail.com", "0502323546","2000-03-03" , 12, "Loran"),
(2, "name2", "somemail12@gmail.com", "0547823546","2002-03-03" , 12, "Loran"),
(3, "name3", "somemail34@gmail.com", "0578963546","2003-03-03" , 12, "Loran"),
(4, "name4", "somemail56@gmail.com", "052334546","2004-03-03" , 12, "Loran")

INSERT INTO Clients
VALUES
(1, "name1", "gonan 13", "0502323546"),
(2, "name2", "hahava 34", "0547823546"),
(3, "name3", "lotem 3", "0578963546"),
(4, "name4", "pituran 45", "052334546")

INSERT INTO Types
VALUES
( "workout1", "des1"),
( "workout2", "des2"),
( "workout3", "des3"),
( "workout4", "des4")

INSERT INTO Coaches
VALUES
( 1,"workout1"),
( 2,"workout2"),
( 3,"workout3"),
( 4,"workout4")


INSERT INTO Training_sequence
VALUES
( 1,"2006-04-04", 1,"workout1",1),
( 2,"2006-04-05",2,"workout2",1),
( 3,"2006-04-06",3,"workout3",1),
( 4,"2006-04-07",4,"workout4",1)
```

## Task 1 - 3.

```
SELECT cl.name, ca.hourly_rate * tr.hours AS sequence_cost
FROM Training_sequence tr
LEFT JOIN Clients cl
ON tr.c_id = cl.c_id
LEFT JOIN Coach ca
ON tr.coach_id = ca.id
```

## Task 2 - 1.

```
CREATE TABLE Event(
etype  VARCHAR(255),
description  TINYTEXT,
PRIMARY KEY (etype)
)

CREATE TABLE City (
cname VARCHAR(255),
country TINYTEXT,
population  INT,
PRIMARY KEY (cname)
)

CREATE TABLE Disaster (
cname VARCHAR(255),
year SMALLINT,
etype VARCHAR(255),
casualties INT,
FOREIGN KEY (cname) REFERENCES City(cname),
FOREIGN KEY (etype) REFERENCES Event(etype)
)

CREATE TABLE Prediction (
cname VARCHAR(255),
etype VARCHAR(255),
casualties INT,
FOREIGN KEY (cname) REFERENCES City(cname),
FOREIGN KEY (etype) REFERENCES Event(etype)
)

CREATE TABLE Measures (
etype VARCHAR(255),
provider VARCHAR(255),
cost INT,
precent DECIMAL,
FOREIGN KEY (etype) REFERENCES Event(etype)
)
```

## Task 2 - 2.

```
insert into event values
("earthquake","from 5 in Richter scale"),
("fire","burning that produces flames that send out heat and light and sometimes smoke"),
("flood","overflowing of a large amount of water over what is normally dry land"),
("tornado","funnel-shaped vortex of violently rotating winds advancing beneath a large storm system"),
("volcano","lava, ash, rock and gasses eruption")

insert into city values
("Hilo","Hawaii",43000),
("Kagoshima","Japan",700000),
("Naples","Italy",3000000),
("Pasto","Columbia",450000),
("Tsfat","Israel",3600)

insert into disaster values
("Hilo","1903","volcano",500),
("Hilo","1914","volcano",300),
("Hilo","1926","volcano",20),
("Hilo","1971","tornado",85),
("Hilo","1984","volcano",50),
("Hilo","1989","tornado",50),
("Hilo","2002","tornado",5),
("Hilo","2011","tornado",70),
("Hilo","2015","tornado",50),
("Kagoshima","1914","earthquake",35),
("Kagoshima","1915","volcano",100),
("Kagoshima","1974","volcano",50),
("Kagoshima","1993","flood",2600),
("Kagoshima","2017","volcano",20),
("Naples","1906","volcano",50),
("Naples","1944","volcano",35),
("Naples","1979","volcano",50),
("Naples","1998","flood",200),
("Pasto","1988","volcano",30),
("Pasto","1993","volcano",45),
("Pasto","2002","volcano",15),
("Pasto","2008","volcano",30),
("Pasto","2010","volcano",30),
("Pasto","2018","flood",15),
("Tsfat","1837","earthquake",5000)

insert into predictions values
("Hilo","flood",100),
("Hilo","tornado",400),
("Kagoshima","fire",320),
("Naples","volcano",50),
("Pasto","tornado",200),
("Pasto","volcano",500),
("Tsfat","earthquake",1000)

insert into measures values
("flood","fire" "department",1000000,60),
("tornado","army",300000,20),
("tornado","police",400000,50),
("volcano","army",600000,90),
("volcano","fire" "department",500000,85),
("volcano","police",200000,70)
```

## Task 2 - 3.

```
SELECT c.*, COUNT(d.cname) AS number_of_disasters
FROM City c LEFT JOIN Disaster d
ON (c.cname = d.cname)
WHERE d.year > 1972
GROUP BY c.cname
ORDER BY number_of_disasters DESC
LIMIT 1
```

## Task 2 - 4.

```
CREATE VIEW Safety_rank AS (
SELECT c.cname, (SUM(p.casualties) / c.population) AS safety
FROM Prediction p LEFT JOIN City c
ON (p.cname = c.cname)
GROUP BY c.cname
ORDER BY safety
)
```
