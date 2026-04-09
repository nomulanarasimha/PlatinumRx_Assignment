# Data Analyst Assignment – PlatinumRx

##  Name:

Narasimha

---

#Project Overview

This project demonstrates skills in:

* SQL (Database & Queries)
* Excel (Data Analysis)
* Python (Programming Logic)

---

#  PHASE 1: SQL – HOTEL DATABASE

## Create Database

```sql
CREATE DATABASE hotel_db;
USE hotel_db;
```

---

##  Create Tables

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(15),
    created_at DATETIME
);

CREATE TABLE bookings (
    booking_id INT PRIMARY KEY,
    booking_date DATE,
    room_no INT,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

CREATE TABLE items (
    items_id INT PRIMARY KEY,
    item_name VARCHAR(200),
    item_rate INT
);

CREATE TABLE booking_commercials (
    id INT PRIMARY KEY,
    booking_id INT,
    bill_id INT,
    bill_date DATE,
    item_id INT,
    item_quantity INT,
    FOREIGN KEY (booking_id) REFERENCES bookings(booking_id) ON DELETE CASCADE
);
```

---

##  Insert Data

```sql
INSERT INTO users VALUES
(1,'Ram','ram@gmail.com','9876543210','2021-11-01 10:00:00'),
(2,'Sam','sam@gmail.com','9876543211','2021-11-02 11:00:00'),
(3,'John','john@gmail.com','9876543212','2021-11-03 12:00:00');

INSERT INTO bookings VALUES
(1,'2021-11-01',101,1),
(2,'2021-11-02',102,2),
(3,'2021-11-03',103,1);

INSERT INTO items VALUES
(1,'Food',200),
(2,'Laundry',100),
(3,'Spa',500);

INSERT INTO booking_commercials VALUES
(1,1,101,'2021-11-01',1,2),
(2,1,102,'2021-11-01',2,1),
(3,2,103,'2021-11-02',3,1);
```

---

##  Queries

### Q1: Last Booked Room

```sql
SELECT room_no
FROM bookings
ORDER BY booking_date DESC
LIMIT 1;
```

---

### Q2: Total Billing

```sql
SELECT b.booking_id, SUM(bc.item_quantity * i.item_rate) AS total_bill
FROM bookings b
JOIN booking_commercials bc ON b.booking_id = bc.booking_id
JOIN items i ON bc.item_id = i.items_id
GROUP BY b.booking_id;
```

---

### Q3: Bills Greater Than 1000

```sql
SELECT b.booking_id, SUM(bc.item_quantity * i.item_rate) AS total_bill
FROM bookings b
JOIN booking_commercials bc ON b.booking_id = bc.booking_id
JOIN items i ON bc.item_id = i.items_id
GROUP BY b.booking_id
HAVING total_bill > 1000;
```

---

### Q4: Most Ordered Item

```sql
SELECT i.item_name, SUM(bc.item_quantity) AS total_qty
FROM booking_commercials bc
JOIN items i ON bc.item_id = i.items_id
GROUP BY i.item_name
ORDER BY total_qty DESC
LIMIT 1;
```

---

### Q5: 2nd Highest Bill

```sql
SELECT total_bill FROM (
    SELECT SUM(bc.item_quantity * i.item_rate) AS total_bill
    FROM booking_commercials bc
    JOIN items i ON bc.item_id = i.items_id
    GROUP BY bc.booking_id
    ORDER BY total_bill DESC
    LIMIT 2
) AS temp
ORDER BY total_bill ASC
LIMIT 1;
```

---

#  PHASE 1: CLINIC DATABASE

## Create Database

```sql
CREATE DATABASE clinic_db;
USE clinic_db;
```

---

## Create Tables & Insert Data

```sql
CREATE TABLE clinic_sales (
    id INT PRIMARY KEY,
    sales_channel VARCHAR(50),
    amount INT,
    sale_date DATE
);

CREATE TABLE expenses (
    id INT PRIMARY KEY,
    expense_amount INT,
    expense_date DATE
);

INSERT INTO clinic_sales VALUES
(1,'Online',1000,'2021-11-01'),
(2,'Offline',2000,'2021-11-02');

INSERT INTO expenses VALUES
(1,500,'2021-11-01'),
(2,700,'2021-11-02');
```

---

##  Queries

### Revenue by Channel

```sql
SELECT sales_channel, SUM(amount) AS revenue
FROM clinic_sales
GROUP BY sales_channel;
```

---

### Profit Calculation

```sql
SELECT 
SUM(cs.amount) AS revenue,
SUM(e.expense_amount) AS expenses,
(SUM(cs.amount) - SUM(e.expense_amount)) AS profit
FROM clinic_sales cs, expenses e;
```

---

#  PHASE 2: EXCEL

* Used VLOOKUP to fetch data
* Compared dates using INT()
* Used HOUR() for time comparison
* Used COUNTIF for analysis

---

#  PHASE 3: PYTHON

## Time Converter

```python
minutes = 130
print(f"{minutes//60} hours and {minutes%60} minutes")
```

---

## Remove Duplicates

```python
s = "programming"
result = ""

for ch in s:
    if ch not in result:
        result += ch

print(result)
