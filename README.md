# BookMyShow Database Design and Queries

This document outlines the database structure and sample queries for a `BookMyShow`-like movie ticket booking system.

---

## **Entities and Attributes**

### **Theater**
- `theater_id` (Primary Key)
- `theater_name`
- `theater_location`

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

### **2. Create Tables**

#### **Theater Table**
```sql
CREATE TABLE Theater (
    theater_id INT PRIMARY KEY,
    theater_name VARCHAR(100),
    theater_location VARCHAR(100)
);
```

#### **Movie Table**
```sql
CREATE TABLE Movie (
    movie_id INT PRIMARY KEY,
    movie_name VARCHAR(100),
    language VARCHAR(100),
    duration INT
);
```

#### **Shows Table**
```sql
CREATE TABLE Shows (
    show_id INT PRIMARY KEY,
    theater_id INT,
    time TIME,
    date DATE,
    movie_id INT,
    FOREIGN KEY (theater_id) REFERENCES Theater(theater_id),
    FOREIGN KEY (movie_id) REFERENCES Movie(movie_id)
);
```

#### **Customer Table**
```sql
CREATE TABLE Customer (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_email VARCHAR(100)
);
```

#### **Booking Table**
```sql
CREATE TABLE Booking (
    booking_id INT PRIMARY KEY,
    customer_id INT,
    show_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    FOREIGN KEY (show_id) REFERENCES Shows(show_id)
);
```

#### **Seat Table**
```sql
CREATE TABLE Seat (
    seat_no INT,
    row_no INT,
    show_id INT,
    booking_id INT,
    PRIMARY KEY (show_id, seat_no, row_no),
    FOREIGN KEY (booking_id) REFERENCES Booking(booking_id)
);
```

---

## **Sample Data**

### **Insert Sample Data**

#### **Theater Data**
```sql
INSERT INTO Theater (theater_id, theater_name, theater_location) VALUES
(1, 'Cineplex', 'Downtown'),
(2, 'Movieland', 'Uptown');
```

#### **Movie Data**
```sql
INSERT INTO Movie (movie_id, movie_name, language, duration) VALUES
(101, 'Inception', 'English', 148),
(102, 'Parasite', 'Korean', 132);
```

#### **Shows Data**
```sql
INSERT INTO Shows (show_id, theater_id, time, date, movie_id) VALUES
(1001, 1, '14:00:00', '2023-12-01', 101),
(1002, 2, '16:00:00', '2023-12-01', 102);
```

#### **Customer Data**
```sql
INSERT INTO Customer (customer_id, customer_name, customer_email) VALUES
(1, 'Alice Johnson', 'alice@example.com'),
(2, 'Bob Smith', 'bob@example.com');
```

#### **Booking Data**
```sql
INSERT INTO Booking (booking_id, customer_id, show_id) VALUES
(5001, 1, 1001),
(5002, 2, 1002);
```

#### **Seat Data**
```sql
INSERT INTO Seat (seat_no, row_no, show_id, booking_id) VALUES
(1, 1, 1001, 5001),
(2, 1, 1002, 5002);
```

---

## **Indexing and Queries**

### **1. Create Index**
To optimize queries involving the `movie_id`:
```sql
CREATE INDEX idx_movie_id ON movie (movie_id);
```

### **2. Query for Shows at a Specific Theater**
Retrieve movie name, language, and time for shows at `Cineplex` on `2023-12-01`:
```sql
SELECT m.movie_name, m.language, s.time
FROM shows s
INNER JOIN movie m ON s.movie_id = m.movie_id
INNER JOIN theater t ON s.theater_id = t.theater_id
WHERE s.date = '2023-12-01'
  AND t.theater_name = 'Cineplex';
```

---

## **Notes**
- This schema is designed for scalability and can be expanded further to include features like:
  - Payment processing
  - Advanced seat selection
  - Customer preferences and reviews.
- Always ensure data integrity with proper constraints and relationships.

---

### **Contact**
For suggestions or improvements, feel free to raise an issue or submit a pull request!

