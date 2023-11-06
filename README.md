# Solving-Real-Life-Business-Problems-using-SQL
## Customers Table
CREATE TABLE Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    location VARCHAR(100)
);

## Orders Table
CREATE TABLE Orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

## Products Table
CREATE TABLE Products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL
);

## Categories Table
CREATE TABLE Categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

## Orde Table 
CREATE TABLE Order_Items (
    order_item_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

### Task: 1
SELECT
    Customers.customer_id,
    Customers.name AS customer_name,
    Customers.email,
    Customers.location,
    COUNT(Orders.order_id) AS total_orders_placed
FROM
    Customers
LEFT JOIN
    Orders ON Customers.customer_id = Orders.customer_id
GROUP BY
    Customers.customer_id, Customers.name, Customers.email, Customers.location
ORDER BY
    total_orders_placed DESC;

### Task: 2
SELECT
    Orders.order_id,
    Products.name AS product_name,
    Order_Items.quantity,
    (Order_Items.quantity * Order_Items.unit_price) AS total_amount
FROM
    Order_Items
JOIN
    Products ON Order_Items.product_id = Products.product_id
JOIN
    Orders ON Order_Items.order_id = Orders.order_id
ORDER BY
    Orders.order_id ASC;

### Task: 3
SELECT
    C.name AS category_name,
    SUM(OI.quantity * OI.unit_price) AS total_revenue
FROM
    Categories C
JOIN
    Products P ON C.category_id = P.category_id
LEFT JOIN
    Order_Items OI ON P.product_id = OI.product_id
GROUP BY
    C.name
ORDER BY
    total_revenue DESC;

### Task: 4
SELECT
    C.name AS customer_name,
    SUM(OI.quantity * OI.unit_price) AS total_purchase_amount
FROM
    Customers C
JOIN
    Orders O ON C.customer_id = O.customer_id
JOIN
    Order_Items OI ON O.order_id = OI.order_id
GROUP BY
    C.name
ORDER BY
    total_purchase_amount DESC
LIMIT 5;
