# goit-rdb-hw-04
домашка по 4 лекції
# 📚 SQL Homework: LibraryManagement & Lesson 3 Joins

![MySQL](https://img.shields.io/badge/MySQL-Workbench-blue)
![SQL](https://img.shields.io/badge/SQL-Practice-green)
![Status](https://img.shields.io/badge/status-completed-success)

---

## 📌 Overview

This repository contains two SQL tasks:

1. **LibraryManagement** database creation with test data.
2. **Lesson 3** SQL queries based on the CSV dataset, including `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, filtering, grouping, aggregation, sorting, and pagination.

---

# Part 1. LibraryManagement

1.1. Create Schema

```sql
CREATE SCHEMA IF NOT EXISTS LibraryManagement;
USE LibraryManagement;
``` 
1.2. Create Table authors
```sql
CREATE TABLE authors (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    author_name VARCHAR(255)
);
``` 
1.3. Create Table genres
```sql
CREATE TABLE genres (
    genre_id INT AUTO_INCREMENT PRIMARY KEY,
    genre_name VARCHAR(255)
);
```
1.4. Create Table books
```sql
CREATE TABLE books (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    publication_year YEAR,
    author_id INT,
    genre_id INT,
    FOREIGN KEY (author_id) REFERENCES authors(author_id),
    FOREIGN KEY (genre_id) REFERENCES genres(genre_id)
);
```

1.5. Create Table users
```sql
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255),
    email VARCHAR(255)
);
```
1.6. Create Table borrowed_books
```sql
CREATE TABLE borrowed_books (
    borrow_id INT AUTO_INCREMENT PRIMARY KEY,
    book_id INT,
    user_id INT,
    borrow_date DATE,
    return_date DATE,
    FOREIGN KEY (book_id) REFERENCES books(book_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```
1.7. Insert Test Data
```sql
INSERT INTO authors (author_name)
VALUES 
('J.K. Rowling'),
('George Orwell');

INSERT INTO genres (genre_name)
VALUES 
('Fantasy'),
('Dystopian');

INSERT INTO books (title, publication_year, author_id, genre_id)
VALUES
('Harry Potter and the Philosopher''s Stone', 1997, 1, 1),
('1984', 1949, 2, 2);

INSERT INTO users (username, email)
VALUES
('john_doe', 'john@example.com'),
('anna_reader', 'anna@example.com');

INSERT INTO borrowed_books (book_id, user_id, borrow_date, return_date)
VALUES
(1, 1, '2024-01-10', '2024-01-20'),
(2, 2, '2024-02-01', NULL);
```
1.8. Check the Data
```sql
SELECT * FROM authors;
SELECT * FROM genres;
SELECT * FROM books;
SELECT * FROM users;
SELECT * FROM borrowed_books;
```
# Part 2. Lesson 3: Dataset Queries
📂 Tables Used
The following tables are used in the Lesson 3 task:
- categories(id, name, description)
- customers(id, name, contact, address, city, postal_code, country)
- employees(employee_id, last_name, first_name, birthdate, photo, notes)
- order_details(id, order_id, product_id, quantity)
- orders(id, customer_id, employee_id, date, shipper_id)
- products(id, name, supplier_id, category_id, unit, price)
- shippers(id, name, phone)
- suppliers(id, name, contact, address, city, postal_code, country, phone)
  
2.1. Join All Tables Using FROM and INNER JOIN
```sql
SELECT *
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id;
```
2.2. Count the Number of Rows
```sql
SELECT COUNT(*) AS total_rows
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id;
```
2.3. Replace INNER JOIN with LEFT JOIN
```sql
SELECT COUNT(*) AS total_rows
FROM order_details
LEFT JOIN orders
    ON order_details.order_id = orders.id
LEFT JOIN customers
    ON orders.customer_id = customers.id
LEFT JOIN products
    ON order_details.product_id = products.id
LEFT JOIN categories
    ON products.category_id = categories.id
LEFT JOIN employees
    ON orders.employee_id = employees.employee_id
LEFT JOIN shippers
    ON orders.shipper_id = shippers.id
LEFT JOIN suppliers
    ON products.supplier_id = suppliers.id;
2.4. Replace INNER JOIN with RIGHT JOIN
SELECT COUNT(*) AS total_rows
FROM order_details
RIGHT JOIN orders
    ON order_details.order_id = orders.id
RIGHT JOIN customers
    ON orders.customer_id = customers.id
RIGHT JOIN products
    ON order_details.product_id = products.id
RIGHT JOIN categories
    ON products.category_id = categories.id
RIGHT JOIN employees
    ON orders.employee_id = employees.employee_id
RIGHT JOIN shippers
    ON orders.shipper_id = shippers.id
RIGHT JOIN suppliers
    ON products.supplier_id = suppliers.id;
```
2.5. Explanation of Row Count Changes

- When an `INNER JOIN` is used, only matching rows from both tables are returned.
- When `LEFT JOIN` is used, all rows from the left table are returned, even if there are no matching rows in the right table. Because of this, the number of rows may increase or remain unchanged.
- When `RIGHT JOIN` is used, all rows from the right table are returned, even if there are no matching rows in the left table. Therefore, the number of rows may also change.
- In summary, `INNER JOIN` returns only matching records, while LEFT JOIN and RIGHT JOIN may also include unmatched records.
2.6. Select Rows Where employee_id > 3 and employee_id <= 10
```sql
SELECT *
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id
WHERE employees.employee_id > 3
  AND employees.employee_id <= 10;
```
2.7. Count Rows for the Filtered Result
```sql
SELECT COUNT(*) AS total_rows
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id
WHERE employees.employee_id > 3
  AND employees.employee_id <= 10;
```
2.8. Group by Category Name, Count Rows, and Find Average Quantity
```sql
SELECT
    categories.name AS category_name,
    COUNT(*) AS row_count,
    AVG(order_details.quantity) AS avg_quantity
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id
WHERE employees.employee_id > 3
  AND employees.employee_id <= 10
GROUP BY categories.name;
```
2.9. Filter Rows Where Average Quantity Is Greater Than 21
```sql
SELECT
    categories.name AS category_name,
    COUNT(*) AS row_count,
    AVG(order_details.quantity) AS avg_quantity
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id
WHERE employees.employee_id > 3
  AND employees.employee_id <= 10
GROUP BY categories.name
HAVING AVG(order_details.quantity) > 21;
```
2.10. Sort Rows by Row Count in Descending Order
```sql
SELECT
    categories.name AS category_name,
    COUNT(*) AS row_count,
    AVG(order_details.quantity) AS avg_quantity
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id
WHERE employees.employee_id > 3
  AND employees.employee_id <= 10
GROUP BY categories.name
HAVING AVG(order_details.quantity) > 21
ORDER BY row_count DESC;
```
2.11. Display 4 Rows, Skipping the First Row
```sql
SELECT
    categories.name AS category_name,
    COUNT(*) AS row_count,
    AVG(order_details.quantity) AS avg_quantity
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id
WHERE employees.employee_id > 3
  AND employees.employee_id <= 10
GROUP BY categories.name
HAVING AVG(order_details.quantity) > 21
ORDER BY row_count DESC
LIMIT 4 OFFSET 1;
```
2.12. Final Query for Task 4
```sql
SELECT
    categories.name AS category_name,
    COUNT(*) AS row_count,
    AVG(order_details.quantity) AS avg_quantity
FROM order_details
INNER JOIN orders
    ON order_details.order_id = orders.id
INNER JOIN customers
    ON orders.customer_id = customers.id
INNER JOIN products
    ON order_details.product_id = products.id
INNER JOIN categories
    ON products.category_id = categories.id
INNER JOIN employees
    ON orders.employee_id = employees.employee_id
INNER JOIN shippers
    ON orders.shipper_id = shippers.id
INNER JOIN suppliers
    ON products.supplier_id = suppliers.id
WHERE employees.employee_id > 3
  AND employees.employee_id <= 10
GROUP BY categories.name
HAVING AVG(order_details.quantity) > 21
ORDER BY row_count DESC
LIMIT 4 OFFSET 1;
```
# ✅ Skills Demonstrated
- Database schema creation
- Table relationships with FOREIGN KEY
- Inserting test data
- Multi-table joins
- Filtering with WHERE
- Aggregation with COUNT and AVG
- Grouping with GROUP BY
- Filtering grouped data with HAVING
- Sorting with ORDER BY
- Pagination with LIMIT and OFFSET
