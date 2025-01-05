# BookMyShow Database Design and Queries

This document outlines the database structure and sample queries for a `BookMyShow`-like movie ticket booking system.

---

## **Entities and Attributes**

### **Theater**
- `theater_id` (Primary Key)
- `theater_name`
- `theater_location`
- `capacity`

### **Show**
- `show_id` (Primary Key)
- `theater_id` (Foreign Key)
- `time`
- `date`
- `movie_id` (Foreign Key)

### **Movie**
- `movie_id` (Primary Key)
- `movie_name`
- `duration`
- `language`

### **Customer**
- `customer_id` (Primary Key)
- `customer_name`
- `customer_email`

### **Seat**
- `seat_no`
- `row_no`
- `show_id` (Primary Key)
- `booking_id` (Foreign Key)

### **Booking**
- `booking_id` (Primary Key)
- `customer_id` (Foreign Key)
- `show_id` (Foreign Key)

---

## **Database Table Structure**

### **1. Create Database**
```sql
CREATE DATABASE bookmyshow;
```
mysql> CREATE DATABASE bookmyshow;
Query OK, 1 row affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| bookmyshow         |
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
+--------------------+
6 rows in set (0.00 sec)
### **2. Create Tables**
mysql> use bookmyshow;
Database changed
#### **Theater Table**
```sql
mysql> CREATE TABLE Theater (
    ->     theater_id INT AUTO_INCREMENT PRIMARY KEY,
    ->     theater_name VARCHAR(255),
    ->     theater_location VARCHAR(255),
    ->     capacity INT, 
    ->     UNIQUE (theater_name, theater_location)
    -> );
Query OK, 0 rows affected (0.04 sec)

mysql> show tables;
+----------------------+
| Tables_in_bookmyshow |
+----------------------+
| theater              |
+----------------------+
1 row in set (0.01 sec)
```


#### **Movie Table**
```sql
mysql> CREATE TABLE Movie (    
    movie_id INT AUTO_INCREMENT PRIMARY KEY,   
    movie_name VARCHAR(255),    
    language VARCHAR(255),    
    duration INT,    
    UNIQUE (movie_name, language));
Query OK, 0 rows affected (0.04 sec)

mysql> show tables;
+----------------------+
| Tables_in_bookmyshow |
+----------------------+
| movie                |
| theater              |
+----------------------+
2 rows in set (0.01 sec)
```

#### **Shows Table**
```sql
mysql> CREATE TABLE Shows (
    ->     show_id INT AUTO_INCREMENT PRIMARY KEY,
    ->     theater_id INT,
    ->     time TIME,
    ->     date DATE,
    ->     movie_id INT,
    ->     FOREIGN KEY (theater_id) REFERENCES Theater(theater_id),
    ->     FOREIGN KEY (movie_id) REFERENCES Movie(movie_id),
    ->     UNIQUE  (theater_id, time, date, movie_id)
    -> );
Query OK, 0 rows affected (0.05 sec)

mysql> show tables;
+----------------------+
| Tables_in_bookmyshow |
+----------------------+
| movie                |
| shows                |
| theater              |
+----------------------+
3 rows in set (0.00 sec)
```

#### **Customer Table**
```sql
mysql> CREATE TABLE Customer (
    ->     customer_id INT AUTO_INCREMENT PRIMARY KEY,
    ->     customer_name VARCHAR(255) UNIQUE,
    ->     customer_email VARCHAR(255) UNIQUE,
    ->     customer_password VARCHAR(255)
    -> );
Query OK, 0 rows affected (0.05 sec)

mysql> SHOW TABLES;
+----------------------+
| Tables_in_bookmyshow |
+----------------------+
| customer             |
| movie                |
| shows                |
| theater              |
+----------------------+
4 rows in set (0.00 sec)
```

#### **Booking Table**
```sql
CREATE TABLE Booking (
    booking_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    show_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    FOREIGN KEY (show_id) REFERENCES Shows(show_id)
);
```

#### **Seat Table**
```sql
mysql> CREATE TABLE Seat (
    ->     seat_no INT,
    ->     row_no INT,
    ->     show_id INT,
    ->     booking_id INT,
    ->     PRIMARY KEY (show_id, seat_no, row_no),
    ->     FOREIGN KEY (booking_id) REFERENCES Booking(booking_id),
    ->     UNIQUE(seat_no,
    ->     row_no,
    ->     show_id)
    -> );
Query OK, 0 rows affected (0.03 sec)

mysql> SHOW TABLES;
+----------------------+
| Tables_in_bookmyshow |
+----------------------+
| booking              |
| customer             |
| movie                |
| seat                 |
| shows                |
| theater              |
+----------------------+
6 rows in set (0.01 sec)
```

---

## **Sample Data**

### **Insert Sample Data**

#### **Theater Data**
```sql
mysql> INSERT INTO Theater (theater_id, theater_name, theater_location, capacity) VALUES
    -> (1, 'Cineplex', 'Downtown', 80),
    -> (2, 'Movieland', 'Uptown', 80);
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from theater;
+------------+--------------+------------------+----------+
| theater_id | theater_name | theater_location | capacity |
+------------+--------------+------------------+----------+
|          1 | Cineplex     | Downtown         |       80 |
|          2 | Movieland    | Uptown           |       80 |
+------------+--------------+------------------+----------+
2 rows in set (0.00 sec)
```

#### **Movie Data**
```sql
mysql> INSERT INTO Movie (movie_id, movie_name, language, duration) VALUES
    -> (101, 'Inception', 'English', 148),
    -> (102, 'Parasite', 'Korean', 132);
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from movie;
+----------+------------+----------+----------+
| movie_id | movie_name | language | duration |
+----------+------------+----------+----------+
|      101 | Inception  | English  |      148 |
|      102 | Parasite   | Korean   |      132 |
+----------+------------+----------+----------+
2 rows in set (0.00 sec)
```

#### **Shows Data**
```sql
mysql> INSERT INTO Shows (show_id, theater_id, time, date, movie_id) VALUES
    -> (1001, 1, '14:00:00', '2023-12-01', 101),
    -> (1002, 2, '16:00:00', '2023-12-01', 102);
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from shows;
+---------+------------+----------+------------+----------+
| show_id | theater_id | time     | date       | movie_id |
+---------+------------+----------+------------+----------+
|    1001 |          1 | 14:00:00 | 2023-12-01 |      101 |
|    1002 |          2 | 16:00:00 | 2023-12-01 |      102 |
+---------+------------+----------+------------+----------+
2 rows in set (0.00 sec)
```

#### **Customer Data**
```sql
mysql> INSERT INTO Customer (customer_id, customer_name, customer_email, customer_password) VALUES
    -> (1, 'Alice Johnson', 'alice@example.com', "123456"),
    -> (2, 'Bob Smith', 'bob@example.com', "456789");
Query OK, 2 rows affected (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from customer;
+-------------+---------------+-------------------+-------------------+
| customer_id | customer_name | customer_email    | customer_password |
+-------------+---------------+-------------------+-------------------+
|           1 | Alice Johnson | alice@example.com | 123456            |
|           2 | Bob Smith     | bob@example.com   | 456789            |
+-------------+---------------+-------------------+-------------------+
2 rows in set (0.00 sec)
```

#### **Booking Data**
```sql
mysql> INSERT INTO Booking (booking_id, customer_id, show_id) VALUES
    -> (5001, 1, 1001),
    -> (5002, 2, 1002);
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from booking;
+------------+-------------+---------+
| booking_id | customer_id | show_id |
+------------+-------------+---------+
|       5001 |           1 |    1001 |
|       5002 |           2 |    1002 |
+------------+-------------+---------+
2 rows in set (0.00 sec)
```

#### **Seat Data**
```sql
mysql> INSERT INTO Seat (seat_no, row_no, show_id, booking_id) VALUES
    -> (1, 1, 1001, 5001),
    -> (2, 1, 1002, 5002);
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from seat;
+---------+--------+---------+------------+
| seat_no | row_no | show_id | booking_id |
+---------+--------+---------+------------+
|       1 |      1 |    1001 |       5001 |
|       2 |      1 |    1002 |       5002 |
+---------+--------+---------+------------+
2 rows in set (0.00 sec)
```

---

## **Indexing and Queries**

### **1. Create Index**
To optimize queries involving the `movie_name`:
```sql
mysql> create index name_movie ON movie(movie_name);
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
```sql
mysql> create index theater_n on theater(theater_name);
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
```
```sql
mysql> create index show_date on shows(date);
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

### **2. Query for Shows at a Specific Theater**
Retrieve movie name, language, and time for shows at `Cineplex` on `2023-12-01`:
```sql
mysql> SELECT m.movie_name, m.language, s.time
    -> FROM shows s
    -> INNER JOIN movie m ON s.movie_id = m.movie_id
    -> INNER JOIN theater t ON s.theater_id = t.theater_id
    -> WHERE s.date = '2023-12-01'
    ->   AND t.theater_name = 'Cineplex';
+------------+----------+----------+
| movie_name | language | time     |
+------------+----------+----------+
| Inception  | English  | 14:00:00 |
+------------+----------+----------+
1 row in set (0.00 sec)
```

---
