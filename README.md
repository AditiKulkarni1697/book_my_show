# book_my_show
Problem Solving Case - BookMyShow

Entities and Attributes:

Theater:
theater_id
theater_name
theater_location

Show:
show_id
theater_id (Foreign Key)
time
date
movie_id (Foreign Key)

Movie:
movie_name
movie_id
duration
language 

Customer:
customer_id
customer_name
customer_email

Seat:
seat_no
row_no
show_id
booking_id (Foreign Key)

Booking:
customer_id (Foreign Key)
show_id (Foreign Key)
booking_id




P1: 
Table Structure:

create database bookmyshow;

CREATE TABLE Theater (
    theater_id INT PRIMARY KEY,
    theater_name VARCHAR(100),
    theater_location VARCHAR(100)
);



CREATE TABLE Movie (
    movie_id INT PRIMARY KEY,
    movie_name VARCHAR(100),
    language VARCHAR(100),
    duration INT
);

CREATE TABLE Shows (
    show_id INT PRIMARY KEY,
    theater_id INT,
    time TIME,
    date DATE,
    movie_id INT,
    FOREIGN KEY (theater_id) REFERENCES Theater(theater_id),
    FOREIGN KEY (movie_id) REFERENCES Movie(movie_id)
);



CREATE TABLE Customer (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_email VARCHAR(100)
);

CREATE TABLE Booking (
    booking_id INT PRIMARY KEY,
    customer_id INT,
    show_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id),
    FOREIGN KEY (show_id) REFERENCES Shows(show_id)
);

CREATE TABLE Seat (
    seat_no INT,
    row_no INT,
    show_id INT,
    booking_id INT,
    PRIMARY KEY (show_id, seat_no, row_no),
    FOREIGN KEY (booking_id) REFERENCES Booking(booking_id)
);



Sample entries: 
INSERT INTO Theater (theater_id, theater_name, theater_location) VALUES
(1, 'Cineplex', 'Downtown'),
(2, 'Movieland', 'Uptown');

INSERT INTO Movie (movie_id, movie_name, language, duration) VALUES
(101, 'Inception', 'English', 148),
(102, 'Parasite', 'Korean', 132);

INSERT INTO Shows (show_id, theater_id, time, date, movie_id) VALUES
(1001, 1, '14:00:00', '2023-12-01', 101),
(1002, 2, '16:00:00', '2023-12-01', 102);

INSERT INTO Customer (customer_id, customer_name, customer_email) VALUES
(1, 'Alice Johnson', 'alice@example.com'),
(2, 'Bob Smith', 'bob@example.com');

INSERT INTO Booking (booking_id, customer_id, show_id) VALUES
(5001, 1, 1001),
(5002, 2, 1002);

INSERT INTO Seat (seat_no, row_no, show_id, booking_id) VALUES
(1, 1, 1001, 5001),
(2, 1, 1002, 5002);


P2:

CREATE INDEX idx_movie_id ON movie (movie_id);

SELECT m.movie_name, m.language, s.time
FROM shows s
INNER JOIN movie m ON s.movie_id = m.movie_id
INNER JOIN theater t ON s.theater_id = t.theater_id
WHERE s.date = '2023-12-01' 
  AND t.theater_name = 'Cineplex';




