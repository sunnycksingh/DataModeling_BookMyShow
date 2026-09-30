# 🎬 BookMyShow Database

A relational database design for a simplified **BookMyShow movie ticket booking system**, including theatres, screens, seats, movies, shows, bookings, and payments.

---

# P1 — Database Design & Sample Data

## 📌 Database Tables

| Table Name | Columns |
|---|---|
| **User** | `user_id (PK)`, `user_name`, `user_phone`, `user_email` |
| **Theatre** | `theatre_id (PK)`, `theatre_name`, `theatre_location` |
| **Screen** | `screen_id (PK)`, `theatre_id (FK)`, `screen_number` |
| **Seat** | `seat_id (PK)`, `screen_id (FK)`, `seat_number`, `seat_type` |
| **Movie** | `movie_id (PK)`, `movie_name`, `movie_duration` |
| **Showdetails** | `show_id (PK)`, `movie_id (FK)`, `screen_id (FK)`, `show_date`, `start_time`, `end_time` |
| **Show_Seat** | `show_id (PK, FK)`, `seat_id (PK, FK)`, `seat_status` |
| **Booking** | `booking_id (PK)`, `user_id (FK)`, `show_id (FK)`, `booking_status`, `created_at` |
| **Booking_Seat** | `booking_id (PK, FK)`, `seat_id (PK, FK)` |
| **Payment** | `payment_id (PK)`, `booking_id (FK)`, `provider_payment_id`, `payment_mode`, `payment_amount`, `payment_status`, `payment_date`, `created_at` |

---

## 🗄️ Table Structure

### User

```sql
CREATE TABLE User (
    user_id INT PRIMARY KEY,
    user_name VARCHAR(50) NOT NULL,
    user_phone VARCHAR(10) NOT NULL UNIQUE,
    user_email VARCHAR(100) NOT NULL UNIQUE
);
```

### Theatre

```sql
CREATE TABLE Theatre (
    theatre_id INT PRIMARY KEY,
    theatre_name VARCHAR(100) NOT NULL,
    theatre_location VARCHAR(100) NOT NULL
);
```

### Screen

```sql
CREATE TABLE Screen (
    screen_id INT PRIMARY KEY,
    theatre_id INT NOT NULL,
    screen_number INT NOT NULL UNIQUE,

    FOREIGN KEY (theatre_id)
        REFERENCES Theatre(theatre_id),

    UNIQUE (theatre_id, screen_number)
);
```

### Movie

```sql
CREATE TABLE Movie (
    movie_id INT PRIMARY KEY,
    movie_name VARCHAR(100) NOT NULL,
    movie_duration INT NOT NULL
);
```

### Seat

```sql
CREATE TABLE Seat (
    seat_id INT PRIMARY KEY,
    screen_id INT NOT NULL,
    seat_number VARCHAR(10) NOT NULL UNIQUE,
    seat_type VARCHAR(20) NOT NULL,

    FOREIGN KEY (screen_id)
        REFERENCES Screen(screen_id),

    UNIQUE (screen_id, seat_number)
);
```

### Showdetails

```sql
CREATE TABLE Showdetails (
    show_id INT PRIMARY KEY,
    movie_id INT NOT NULL,
    screen_id INT NOT NULL,
    show_date DATE NOT NULL,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,

    FOREIGN KEY (movie_id)
        REFERENCES Movie(movie_id),

    FOREIGN KEY (screen_id)
        REFERENCES Screen(screen_id)
);
```

### Show_Seat

```sql
CREATE TABLE Show_Seat (
    show_id INT NOT NULL,
    seat_id INT NOT NULL,
    seat_status VARCHAR(20) NOT NULL,

    PRIMARY KEY (show_id, seat_id),

    FOREIGN KEY (show_id)
        REFERENCES Showdetails(show_id),

    FOREIGN KEY (seat_id)
        REFERENCES Seat(seat_id)
);
```

### Booking

```sql
CREATE TABLE Booking (
    booking_id INT PRIMARY KEY,
    user_id INT NOT NULL,
    show_id INT NOT NULL,
    booking_status VARCHAR(20) NOT NULL,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (user_id)
        REFERENCES User(user_id),

    FOREIGN KEY (show_id)
        REFERENCES Showdetails(show_id)
);
```

### Booking_Seat

```sql
CREATE TABLE Booking_Seat (
    booking_id INT NOT NULL,
    seat_id INT NOT NULL,

    PRIMARY KEY (booking_id, seat_id),

    FOREIGN KEY (booking_id)
        REFERENCES Booking(booking_id),

    FOREIGN KEY (seat_id)
        REFERENCES Seat(seat_id)
);
```

### Payment

```sql
CREATE TABLE Payment (
    payment_id INT PRIMARY KEY,
    booking_id INT NOT NULL,
    provider_payment_id VARCHAR(100) NOT NULL UNIQUE,
    payment_mode VARCHAR(30) NOT NULL,
    payment_amount DECIMAL(10,2) NOT NULL,
    payment_status VARCHAR(20) NOT NULL,
    payment_date DATETIME,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (booking_id)
        REFERENCES Booking(booking_id)
);
```

---

# 📊 Sample Data

## Users

```sql
INSERT INTO User VALUES
(1, 'Ms Dhoni', '9876543210', 'dhoni@gmail.com'),
(2, 'Taylor Swift', '9876543211', 'taylor@gmail.com');
```

## Theatres

```sql
INSERT INTO Theatre VALUES
(1, 'PVR Phoenix', 'Mumbai'),
(2, 'INOX Malad', 'Mumbai');
```

## Screens

```sql
INSERT INTO Screen VALUES
(101, 1, 1),
(102, 1, 2),
(103, 2, 1);
```

## Movies

```sql
INSERT INTO Movie VALUES
(1, 'Avengers Endgame', 181),
(2, 'Avatar', 192);
```

## Seats

```sql
INSERT INTO Seat VALUES
(1, 101, 'A1', 'REGULAR'),
(2, 101, 'A2', 'REGULAR'),
(3, 101, 'A3', 'PREMIUM'),
(4, 102, 'A1', 'REGULAR'),
(5, 102, 'A2', 'REGULAR');
```

## Shows

```sql
INSERT INTO Showdetails VALUES
(1, 1, 101, '2026-09-30', '10:00:00', '13:01:00'),
(2, 1, 101, '2026-09-30', '18:00:00', '21:01:00'),
(3, 2, 102, '2026-09-30', '14:00:00', '17:12:00');
```

## Show Seats

```sql
INSERT INTO Show_Seat VALUES
(1, 1, 'AVAILABLE'),
(1, 2, 'AVAILABLE'),
(1, 3, 'AVAILABLE'),
(2, 1, 'AVAILABLE'),
(2, 2, 'AVAILABLE'),
(2, 3, 'AVAILABLE'),
(3, 4, 'AVAILABLE'),
(3, 5, 'AVAILABLE');
```

## Bookings

```sql
INSERT INTO Booking VALUES
(1, 1, 1, 'CONFIRMED', '2026-09-30 09:15:00'),
(2, 2, 3, 'CONFIRMED', '2026-09-30 12:30:00');
```

## Booking Seats

```sql
INSERT INTO Booking_Seat VALUES
(1, 1),
(1, 2),
(2, 4);
```

## Payments

```sql
INSERT INTO Payment VALUES
(1, 1, 'PAY10001', 'UPI', 600.00, 'SUCCESS',
 '2026-09-30 09:16:00', '2026-09-30 09:16:00'),

(2, 2, 'PAY10002', 'CARD', 300.00, 'SUCCESS',
 '2026-09-30 12:31:00', '2026-09-30 12:31:00');
```

---

# P2 — Queries & High-Concurrency Booking

## 🎬 List Shows for a Given Theatre and Date

```sql
SELECT 
    sd.show_id,
    m.movie_name,
    sd.show_date,
    sd.start_time,
    sd.end_time
FROM Showdetails sd
JOIN Movie m 
    ON sd.movie_id = m.movie_id
JOIN Screen s 
    ON sd.screen_id = s.screen_id
JOIN Theatre t 
    ON s.theatre_id = t.theatre_id
WHERE sd.show_date = '2026-09-30'
  AND t.theatre_name = 'PVR Phoenix';
```

---

## 🔒 Prevent Double Booking

```sql
BEGIN TRANSACTION;

-- Lock the requested seats for this show
SELECT seat_id, seat_status
FROM Show_Seat WITH (UPDLOCK, HOLDLOCK)
WHERE show_id = 1
  AND seat_id IN (1, 2);

-- Update only available seats
UPDATE Show_Seat
SET seat_status = 'BOOKED'
WHERE show_id = 1
  AND seat_id IN (1, 2)
  AND seat_status = 'AVAILABLE';

-- Create booking
INSERT INTO Booking
    (booking_id, user_id, show_id, booking_status)
VALUES
    (3, 1, 1, 'CONFIRMED');

-- Assign seats to booking
INSERT INTO Booking_Seat
    (booking_id, seat_id)
VALUES
    (3, 1),
    (3, 2);

COMMIT TRANSACTION;
```

---

## ⏳ Seat Hold with Timeout

```sql
BEGIN TRANSACTION;

-- Lock the requested seat
SELECT seat_id, seat_status
FROM Show_Seat WITH (UPDLOCK, HOLDLOCK)
WHERE show_id = 1
  AND seat_id = 3
  AND seat_status = 'AVAILABLE';

-- Hold the seat
UPDATE Show_Seat
SET seat_status = 'HELD'
WHERE show_id = 1
  AND seat_id = 3
  AND seat_status = 'AVAILABLE';

-- Create temporary booking
INSERT INTO Booking
    (booking_id, user_id, show_id, booking_status)
VALUES
    (4, 2, 1, 'HELD');

-- Assign seat to the held booking
INSERT INTO Booking_Seat
    (booking_id, seat_id)
VALUES
    (4, 3);

COMMIT TRANSACTION;
```

---

## ♻️ Release Expired Holds

```sql
UPDATE ss
SET ss.seat_status = 'AVAILABLE'
FROM Show_Seat ss
JOIN Booking_Seat bs
    ON ss.seat_id = bs.seat_id
JOIN Booking b
    ON bs.booking_id = b.booking_id
WHERE b.booking_status = 'HELD'
  AND b.created_at < DATEADD(MINUTE, -10, CURRENT_TIMESTAMP);


UPDATE Booking
SET booking_status = 'EXPIRED'
WHERE booking_status = 'HELD'
  AND created_at < DATEADD(MINUTE, -10, CURRENT_TIMESTAMP);
```

---

## 🔐 Concurrency Strategy

The booking flow uses:

- `UPDLOCK` to lock selected seats during booking.
- `HOLDLOCK` to prevent concurrent transactions from modifying the locked seat rows.
- Transactions using `BEGIN TRANSACTION` and `COMMIT TRANSACTION`.
- `Show_Seat` composite primary key `(show_id, seat_id)` to uniquely identify a seat for a particular show.
- `seat_status` to maintain `AVAILABLE`, `HELD`, and `BOOKED` states.
- Expiry logic to release seats that remain held beyond the timeout period.

> **Note:** The locking syntax shown above uses **SQL Server** syntax (`UPDLOCK`, `HOLDLOCK`). The rest of the database queries may need minor syntax changes if the database is implemented using MySQL.
