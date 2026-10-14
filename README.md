CREATE DATABASE IF NOT EXISTS assignment_db;
USE assignment_db;

-- BANKING TABLES
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    city VARCHAR(50)
);

CREATE TABLE accounts (
    account_id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    account_type VARCHAR(20),
    balance DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO customers (name, city) VALUES
('bhanu','Chennai'),
('pravallika','Coimbatore'),
('Priya','Madurai'),
('Karthik','Salem');

INSERT INTO accounts (customer_id, account_type, balance) VALUES
(1,'Savings',50000),
(1,'Current',20000),
(2,'Savings',30000),
(3,'Savings',15000),
(4,'Current',40000);

-- BANKING QUERIES 1-15
SELECT * FROM accounts WHERE balance > 20000;
SELECT * FROM customers WHERE city = 'Chennai';
SELECT * FROM accounts WHERE balance BETWEEN 20000 AND 50000;
SELECT * FROM customers WHERE name LIKE 'J%';
SELECT * FROM accounts WHERE account_type IN ('Savings','Current');
SELECT * FROM accounts WHERE account_type <> 'Savings';
SELECT * FROM customers WHERE name LIKE '%a%';
SELECT * FROM accounts WHERE balance <= 30000;
SELECT * FROM customers WHERE city <> 'Madurai';
SELECT * FROM accounts WHERE balance NOT BETWEEN 10000 AND 40000;
SELECT * FROM customers WHERE name LIKE '%i';
SELECT * FROM accounts WHERE balance = 50000;
SELECT * FROM customers WHERE city IN ('Chennai','Salem');
SELECT * FROM accounts WHERE balance > 10000 AND balance < 40000;
SELECT * FROM accounts WHERE account_type NOT IN ('Current');

-- BANKING QUERIES 16-28
SELECT * FROM accounts ORDER BY balance DESC;
SELECT * FROM customers ORDER BY name ASC;
SELECT * FROM accounts ORDER BY account_type, balance DESC;
SELECT SUM(balance) AS total_balance FROM accounts;
SELECT AVG(balance) AS average_balance FROM accounts;
SELECT MAX(balance) AS maximum_balance FROM accounts;
SELECT MIN(balance) AS minimum_balance FROM accounts;
SELECT COUNT(*) AS total_customers FROM customers;

SELECT account_type, SUM(balance) AS total_balance
FROM accounts GROUP BY account_type;

SELECT account_type, AVG(balance) AS average_balance
FROM accounts GROUP BY account_type;

SELECT account_type, AVG(balance) AS average_balance
FROM accounts GROUP BY account_type
HAVING AVG(balance) > 20000;

SELECT customer_id, COUNT(*) AS account_count
FROM accounts GROUP BY customer_id;

SELECT c.name, COUNT(a.account_id) AS account_count
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
GROUP BY c.customer_id, c.name
HAVING COUNT(a.account_id) > 1;

-- BANKING QUERIES 29-40
SELECT c.name, a.balance
FROM customers c JOIN accounts a
ON c.customer_id = a.customer_id;

SELECT c.name, a.account_type, a.balance
FROM customers c LEFT JOIN accounts a
ON c.customer_id = a.customer_id;

SELECT a.*, c.name, c.city
FROM accounts a JOIN customers c
ON a.customer_id = c.customer_id;

SELECT c.name, a.account_type
FROM customers c JOIN accounts a
ON c.customer_id = a.customer_id
WHERE a.balance > 20000;

SELECT c.name, SUM(a.balance) AS total_balance
FROM customers c JOIN accounts a
ON c.customer_id = a.customer_id
GROUP BY c.customer_id, c.name;

SELECT c.name, a.balance
FROM customers c JOIN accounts a
ON c.customer_id = a.customer_id
ORDER BY a.balance;

SELECT c.city, COUNT(a.account_id) AS account_count
FROM customers c LEFT JOIN accounts a
ON c.customer_id = a.customer_id
GROUP BY c.city;

SELECT * FROM accounts
WHERE balance > (SELECT AVG(balance) FROM accounts);

SELECT * FROM customers
WHERE customer_id IN (SELECT customer_id FROM accounts);

SELECT * FROM customers
WHERE customer_id NOT IN (SELECT customer_id FROM accounts);

SELECT * FROM accounts
WHERE balance = (SELECT MAX(balance) FROM accounts);

SELECT c.name, SUM(a.balance) AS total_balance
FROM customers c JOIN accounts a
ON c.customer_id = a.customer_id
GROUP BY c.customer_id, c.name
HAVING SUM(a.balance) > 40000;


-- RAILWAY TABLES
CREATE TABLE trains (
    train_id INT PRIMARY KEY AUTO_INCREMENT,
    train_name VARCHAR(50),
    source VARCHAR(50),
    destination VARCHAR(50)
);

CREATE TABLE bookings (
    booking_id INT PRIMARY KEY AUTO_INCREMENT,
    train_id INT,
    passenger_name VARCHAR(50),
    fare DECIMAL(10,2),
    status VARCHAR(20),
    FOREIGN KEY (train_id) REFERENCES trains(train_id)
);

INSERT INTO trains (train_name, source, destination) VALUES
('Express1','Chennai','Madurai'),
('Express2','Coimbatore','Salem'),
('Express3','Madurai','Chennai');

INSERT INTO bookings (train_id, passenger_name, fare, status) VALUES
(1,'Janani',500,'Confirmed'),
(1,'Arun',500,'Waiting'),
(2,'Priya',300,'Confirmed'),
(3,'Karthik',450,'Cancelled'),
(2,'Meena',300,'Confirmed');

-- RAILWAY QUERIES 41-50
SELECT * FROM bookings WHERE fare > 400;
SELECT * FROM bookings WHERE status <> 'Confirmed';
SELECT * FROM trains WHERE source = 'Chennai';
SELECT * FROM bookings WHERE fare BETWEEN 300 AND 500;
SELECT * FROM bookings WHERE passenger_name LIKE 'A%';

SELECT t.train_name, b.passenger_name
FROM trains t JOIN bookings b ON t.train_id = b.train_id;

SELECT t.train_name, COUNT(b.booking_id) AS booking_count
FROM trains t LEFT JOIN bookings b ON t.train_id = b.train_id
GROUP BY t.train_id, t.train_name;

SELECT t.train_name,
       SUM(CASE WHEN b.status = 'Confirmed'
                THEN b.fare ELSE 0 END) AS total_fare
FROM trains t LEFT JOIN bookings b ON t.train_id = b.train_id
GROUP BY t.train_id, t.train_name;

SELECT * FROM bookings
WHERE fare = (SELECT MAX(fare) FROM bookings);

SELECT t.train_name, COUNT(b.booking_id) AS booking_count
FROM trains t JOIN bookings b ON t.train_id = b.train_id
GROUP BY t.train_id, t.train_name
HAVING COUNT(b.booking_id) > 1;


-- EMPLOYEE TABLE
CREATE TABLE Employee (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    department VARCHAR(30),
    salary DECIMAL(10,2),
    city VARCHAR(30),
    joining_date DATE
);

INSERT INTO Employee VALUES
(101,'John','IT',60000,'Chennai','2022-01-15'),
(102,'David','HR',45000,'Bangalore','2021-03-10'),
(103,'Smith','IT',70000,'Chennai','2020-07-12'),
(104,'Mary','Finance',55000,'Mumbai','2023-01-20'),
(105,'James','HR',48000,'Delhi','2022-05-05'),
(106,'Linda','Finance',65000,'Mumbai','2021-08-18');

-- EMPLOYEE QUERIES 1-15
SELECT department, COUNT(*) AS employee_count
FROM Employee GROUP BY department;

SELECT department, AVG(salary) AS average_salary
FROM Employee GROUP BY department;

SELECT department, COUNT(*) AS employee_count
FROM Employee GROUP BY department HAVING COUNT(*) > 1;

SELECT department, MAX(salary) AS highest_salary
FROM Employee GROUP BY department;

SELECT department, MIN(salary) AS lowest_salary
FROM Employee GROUP BY department;

SELECT department, AVG(salary) AS average_salary
FROM Employee GROUP BY department HAVING AVG(salary) > 50000;

SELECT department, SUM(salary) AS total_salary
FROM Employee GROUP BY department;

SELECT * FROM Employee ORDER BY salary DESC;

SELECT * FROM Employee
ORDER BY department, salary DESC;

SELECT city, COUNT(*) AS employee_count
FROM Employee GROUP BY city HAVING COUNT(*) > 1;

SELECT city, SUM(salary) AS total_salary
FROM Employee GROUP BY city;

SELECT department, SUM(salary) AS total_salary
FROM Employee GROUP BY department ORDER BY total_salary DESC;

SELECT department, COUNT(*) AS employee_count
FROM Employee WHERE salary > 50000 GROUP BY department;

SELECT department, MAX(salary) - MIN(salary) AS salary_difference
FROM Employee GROUP BY department;

SELECT * FROM Employee ORDER BY salary DESC LIMIT 3;


-- CUSTOMERS AND ORDERS TABLES
CREATE DATABASE IF NOT EXISTS customer_orders_practice;
USE customer_orders_practice;

CREATE TABLE IF NOT EXISTS Customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50),
    city VARCHAR(30)
);

CREATE TABLE IF NOT EXISTS Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    amount DECIMAL(10,2),
    order_date DATE,
    FOREIGN KEY (customer_id)
        REFERENCES Customers(customer_id)
);

INSERT IGNORE INTO Customers VALUES
(1, 'Anil', 'Hyderabad'),
(2, 'Bhavana', 'Chennai'),
(3, 'Charan', 'Bengaluru'),
(4, 'Divya', 'Vijayawada'),
(5, 'Farah', 'Hyderabad');

INSERT IGNORE INTO Orders VALUES
(101, 1, 3000, '2026-01-10'),
(102, 1, 4500, '2026-02-11'),
(103, 2, 2500, '2026-03-12'),
(104, 3, 6000, '2026-04-13'),
(105, 4, 7000, '2026-05-14'),
(106, 5, 12000, '2026-06-15');

-- Check Customers table
SELECT * FROM Customers;

-- Check Orders table
SELECT * FROM Orders;

-- Question 16: Total order amount for each customer
SELECT
    c.customer_id,
    c.customer_name,
    c.city,
    COALESCE(SUM(o.amount), 0) AS total_amount
FROM Customers c
LEFT JOIN Orders o
    ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name, c.city;
-- CUSTOMERS AND ORDERS QUERIES 16-30
SELECT c.customer_name, COALESCE(SUM(o.amount),0) AS total_amount
FROM Customers c LEFT JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

SELECT c.customer_name, COUNT(o.order_id) AS order_count
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 3;

SELECT c.customer_name, AVG(o.amount) AS average_amount
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

SELECT c.customer_name, MAX(o.amount) AS highest_order
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

SELECT c.customer_name, SUM(o.amount) AS total_purchase
FROM Customers c LEFT JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_purchase DESC;

SELECT c.customer_name, SUM(o.amount) AS total_purchase
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(o.amount) > 10000;

SELECT c.customer_name, COUNT(o.order_id) AS total_orders
FROM Customers c LEFT JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

SELECT c.customer_name, SUM(o.amount) AS total_spent
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_spent DESC LIMIT 1;

SELECT c.customer_name, COUNT(o.order_id) AS order_count
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY order_count DESC LIMIT 1;

SELECT c.customer_name, AVG(o.amount) AS average_amount
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING AVG(o.amount) > 2000;

SELECT c.customer_name, SUM(o.amount) AS total_purchase
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_purchase DESC LIMIT 5;

SELECT c.customer_name, MIN(o.amount) AS minimum_order
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

SELECT c.customer_name, SUM(o.amount) AS total_purchase
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING SUM(o.amount) > 5000;

SELECT c.customer_name, COUNT(o.order_id) AS total_orders,
       COALESCE(SUM(o.amount),0) AS total_purchase
FROM Customers c LEFT JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name;

SELECT c.customer_name, COUNT(o.order_id) AS total_orders,
       SUM(o.amount) AS total_purchase
FROM Customers c JOIN Orders o
ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 2 AND SUM(o.amount) > 8000;

-- STUDENTS TABLE

USE jdbc_db;

CREATE TABLE IF NOT EXISTS Students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(50),
    department VARCHAR(30),
    marks INT
);

INSERT INTO Students
(student_id, student_name, department, marks)
VALUES
(1, 'Asha', 'CSE', 88),
(2, 'Balu', 'CSE', 76),
(3, 'Chaitra', 'CSE', 95),
(4, 'Deepak', 'CSE', 68),
(5, 'Esha', 'CSE', 82);

SELECT * FROM Students;

INSERT INTO Students VALUES
(1,'Asha','CSE',88),
(2,'Balu','CSE',76),
(3,'Chitra','CSE',95),
(4,'Deepak','CSE',68),
(5,'Esha','CSE',82),
(6,'Farhan','CSE',91),
(7,'Gita','ECE',78),
(8,'Hari','ECE',84),
(9,'Indu','ECE',72),
(10,'Jai','ECE',93),
(11,'Kiran','ECE',66),
(12,'Latha','ECE',89),
(13,'Manoj','IT',97),
(14,'Nisha','IT',87),
(15,'Om','IT',79),
(16,'Pooja','IT',92),
(17,'Ravi','IT',74),
(18,'Sita','IT',81);

-- STUDENT QUERIES 31
SELECT department, AVG(marks) AS average_marks
FROM Students GROUP BY department;
-- STUDENT QUERIES 32

SELECT department, AVG(marks) AS average_marks
FROM Students GROUP BY department HAVING AVG(marks) > 75;
-- STUDENT QUERIES 33
SELECT department, MAX(marks) AS highest_mark
FROM Students GROUP BY department;
-- STUDENT QUERIES 34
SELECT department, COUNT(*) AS student_count
FROM Students GROUP BY department;

SELECT department, COUNT(*) AS student_count
FROM Students GROUP BY department HAVING COUNT(*) > 5;

SELECT department, AVG(marks) AS average_marks
FROM Students GROUP BY department ORDER BY average_marks DESC;

SELECT department, AVG(marks) AS average_marks
FROM Students GROUP BY department ORDER BY average_marks DESC LIMIT 3;

SELECT department, AVG(marks) AS average_marks
FROM Students GROUP BY department HAVING AVG(marks) BETWEEN 70 AND 90;

SELECT department, SUM(marks) AS total_marks
FROM Students GROUP BY department;

SELECT department, COUNT(*) AS student_count
FROM Students GROUP BY department ORDER BY student_count;

SELECT department, MIN(marks) AS lowest_mark
FROM Students GROUP BY department;

SELECT department, MAX(marks) AS highest_mark
FROM Students GROUP BY department HAVING MAX(marks) > 90;

SELECT department, COUNT(*) AS students_above_80
FROM Students WHERE marks > 80 GROUP BY department;

SELECT department, COUNT(*) AS students_above_75
FROM Students WHERE marks > 75 GROUP BY department HAVING COUNT(*) > 3;

SELECT department, MAX(marks) AS highest_mark
FROM Students GROUP BY department ORDER BY highest_mark DESC;

USE assignment_db;

-- 1. Banking
SELECT * FROM customers;
SELECT * FROM accounts;

-- 2. Railway
SELECT * FROM trains;
SELECT * FROM bookings;

-- 3. Employee
SELECT * FROM Employee;

-- 4. Customers & Orders
SELECT * FROM Customers;
SELECT * FROM Orders;

-- 5. Students
USE assignment_db;

SELECT * FROM Students;
SHOW TABLES;
