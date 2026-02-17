# OnineBookStore-Project-SQL
SQL project analyzing an online bookstore database with queries for insights and reporting.

## Overview
This project is a relational database for an Online Bookstore built using SQL.  
It includes table creation, data insertion, and advanced analytical queries using joins, aggregate functions, HAVING clauses, and window functions.

The goal of this project is to simulate a real-world bookstore system and extract business insights from the data.

---

## Objectives
- Design a relational database for an online bookstore
- Create relationships between tables
- Perform data analysis using SQL queries
- Apply advanced SQL concepts like JOINs, HAVING, and Window Functions

---

## Database Tables
The database includes the following core tables:

- Customers
- Books
- Orders
- Order_Items

-- Create Database
CREATE DATABASE OnlineBookstore;

-- Create Tables
DROP TABLE IF EXISTS Books;
CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10, 2),
    Stock INT
);
DROP TABLE IF EXISTS customers;
CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);
DROP TABLE IF EXISTS orders;
CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10, 2)
);

SELECT * FROM Books;
SELECT * FROM Customers;
SELECT * FROM Orders;

-- Import Data into Books Table
COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock) 
FROM 'C:\csv\Books.csv' 
CSV HEADER;

-- Import Data into Customers Table
COPY Customers(Customer_ID, Name, Email, Phone, City, Country) 
FROM 'C:\csv\Customers.csv' 
CSV HEADER;

-- Import Data into Orders Table
COPY Orders(Order_ID, Customer_ID, Book_ID, Order_Date, Quantity, Total_Amount) 
FROM 'C:\csv\Orders.csv' 
CSV HEADER;

-- 1) Retrieve all books in the "Fiction" genre:
SELECT*FROM Books
WHERE genre = 'Fiction';



-- 2) Find books published after the year 1950:

SELECT*FROM Books 
WHERE published_year>1950;


-- 3) List all customers from the Canada:

SELECT*FROM Customers
WHERE country = 'Canada';


-- 4) Show orders placed in November 2023:

SELECT * FROM Orders 
WHERE order_date BETWEEN '2023-11-01' AND '2023-11-30';


-- 5) Retrieve the total stock of books available:
SELECT SUM (stock)AS TOTAL_STOCK 
FROM Books;

-- 6) Find the details of the most expensive book:
SELECT* FROM Books 
ORDER BY price DESC 
LIMIT 1;

-- 7) Show all customers who ordered more than 1 quantity of a book:

SELECT*FROM Orders
WHERE quantity >1;


-- 8) Retrieve all orders where the total amount exceeds $20:

SELECT*FROM Orders
WHERE total_amount > 20;

-- 9) List all genres available in the Books table:

SELECT DISTINCT genre FROM Books;

-- 10) Find the book with the lowest stock:
SELECT* FROM Books 
ORDER BY stock ASC 
LIMIT 1;




-- 11) Calculate the total revenue generated from all orders:

SELECT SUM(total_amount) AS REVENUE FROM Orders;

-- Advance Questions : 

-- 1) Retrieve the total number of books sold for each genre:
SELECT*FROM Orders;

SELECT b. genre, SUM(o. Quantity)  AS total_books_sold
FROM Orders o
JOIN Books b ON o.book_id = b.book_id
GROUP BY b. genre;



-- 2) Find the average price of books in the "Fantasy" genre:

SELECT AVG (price)AS AVERAGE_PRICE FROM Books
WHERE genre='Fantasy';



-- 3) List customers who have placed at least 2 orders:

SELECT o.customer_id, c.name, COUNT(o.Order_id)AS ORDER_COUNT
FROM orders o
JOIN customers c ON o.customer_id=c.customer_id
GROUP BY o.customer_id, c.name
HAVING COUNT (Order_id)>=2;

-- 4) Find the most frequently ordered book:
SELECT o.Book_id, b.title, COUNT(o.order_id) AS ORDER_COUNT
FROM orders o
JOIN books b ON o.book_id=b.book_id
GROUP BY o.book_id, b.title
ORDER BY ORDER_COUNT DESC LIMIT 1;

-- 5) Show the top 3 most expensive books of 'Fantasy' Genre :

SELECT* FROM Books 
WHERE genre = 'Fantasy'
ORDER BY price DESC 
LIMIT 3;



-- 6) Retrieve the total quantity of books sold by each author:
SELECT b.author, SUM(o.quantity) AS Total_Books_Sold
FROM orders o
JOIN books b ON o.book_id=b.book_id
GROUP BY b.Author;


-- 7) List the cities where customers who spent over $30 are located:
SELECT DISTINCT c.city, total_amount
FROM orders o
JOIN customers c ON o.customer_id=c.customer_id
WHERE o.total_amount > 30;


-- 8) Find the customer who spent the most on orders:
SELECT c.customer_id, c.name, SUM(o.total_amount) AS Total_Spent
FROM orders o
JOIN customers c ON o.customer_id=c.customer_id
GROUP BY c.customer_id, c.name
ORDER BY Total_spent Desc LIMIT 1;


--9) Calculate the stock remaining after fulfilling all orders:


SELECT b.book_id, b.title, b.stock, COALESCE(SUM(o.quantity),0) AS Order_quantity,  
	b.stock- COALESCE(SUM(o.quantity),0) AS Remaining_Quantity
FROM books b
LEFT JOIN orders o ON b.book_id=o.book_id
GROUP BY b.book_id ORDER BY b.book_id;
[OnlineBookstore Project SQL.sql](https://github.com/user-attachments/files/25363608/OnlineBookstore.Project.SQL.sql)
These tables are connected using primary and foreign key relationships.


## Key SQL Concepts Used
### Basic Queries
- SELECT statements
- INSERT INTO
- Filtering with WHERE

### Intermediate Concepts
- INNER JOIN
- LEFT JOIN
- GROUP BY
- ORDER BY
- Aggregate functions (COUNT, SUM, AVG)

### Advanced Concepts
- HAVING clause
- Subqueries
- Window functions:
  - RANK()
  - ROW_NUMBER()
  - SUM() OVER()
  - PARTITION BY

---

## Sample Analytical Queries
- Top-selling books
- Customers with highest spending
- Monthly sales analysis
- Orders above average value
- Ranking books by sales
- Department/category-wise revenue

---

## Tools Used
- PostgreSQL
- pgAdmin
- SQL

---

## Project Structure
OnlineBookstore-SQL-Project
│
├── create_database.sql
├── create_tables.sql
├── insert_data.sql
├── advanced_queries.sql
├── README.md
└── screenshots/

## Screenshots
Creating tables : 
<img width="1917" height="1077" alt="Screenshot 2026-02-17 163202" src="https://github.com/user-attachments/assets/c30771cb-b3b5-4973-9714-0d728fa0346d" />

<img width="1918" height="1079" alt="Screenshot 2026-02-17 163221" src="https://github.com/user-attachments/assets/df4cc89b-b827-4471-9ebc-3c20c9588dc8" />

<img width="1915" height="1072" alt="Screenshot 2026-02-17 163233" src="https://github.com/user-attachments/assets/0ffce030-2b38-4f7a-b301-3d85f961ecf8" />

Some Queries:
<img width="1906" height="1076" alt="Screenshot 2026-02-17 163256" src="https://github.com/user-attachments/assets/1dde1f8b-5158-4374-9846-a41db4e9f8ef" />

<img width="1919" height="1079" alt="Screenshot 2026-02-17 163448" src="https://github.com/user-attachments/assets/39acf49d-83c9-40ec-adf7-8c51b5588989" />

<img width="1918" height="1079" alt="Screenshot 2026-02-17 163504" src="https://github.com/user-attachments/assets/73966328-1ff7-4255-84c7-f4a1bca09e62" />

<img width="1914" height="1076" alt="Screenshot 2026-02-17 163531" src="https://github.com/user-attachments/assets/4d71c097-b628-4cf0-bd14-cc4d3569c08c" />


## Learning Outcomes
Through this project, I learned:
- How to design a relational database
- How to use joins to combine data from multiple tables
- How to apply HAVING and window functions for analysis
- How to write complex SQL queries for business insights

---

## About Me
I am an aspiring Data Analyst currently learning SQL, Python, and data analytics tools.  
I am actively looking for data analytics internships and entry-level opportunities.

---

## Connect with Me
- LinkedIn:[ https://www.linkedin.com/in/vaishnavi-jadhav-879750344](url)
- GitHub: [https://github.com/Vaishujadhav17](url)
