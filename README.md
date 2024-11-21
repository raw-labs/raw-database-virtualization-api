# RAW Data Virtualization API Template

## Table of Contents

1. [Introduction](#introduction)
   - [Description](#description)
   - [How It Works](#how-it-works)
   - [Features](#features)
2. [Getting Started](#getting-started)
3. [Domain Entities](#domain-entities)
4. [Endpoint Overview](#endpoint-overview)
5. [Query Structure](#query-structure)
   - [Basic Structure of SQL Files](#basic-structure-of-sql-files)
   - [Using Variables](#using-variables)
6. [Customization](#customization)
7. [Example SQL Queries](#example-sql-queries)
8. [Support and Troubleshooting](#support-and-troubleshooting)
9. [License](#license)
10. [Acknowledgements](#acknowledgements)
11. [Contact](#contact)
12. [Additional Resources](#additional-resources)

---

## Introduction

### Description

This repository provides a **Data Virtualization API Template** using the RAW platform. It demonstrates how to create API endpoints that combine data from multiple backend systems into a single response using SQL queries. The central point of this template is to showcase RAW's ability to perform data virtualization by integrating data from various sources in a single query. This is ideal for testing, prototyping, or educational purposes where actual database connections are not required.

### How It Works

The RAW platform allows you to create APIs by writing SQL queries that can access and combine data from various data sources. In this template, we simulate multiple backend systems using the `VALUES` clause in SQL to create inline tables with hardcoded data. The endpoints accept optional query parameters, which are injected into the SQL queries using the `:<variable_name>` notation, as per the RAW platform's convention.

**Key Point:** The `/inventory/stock` endpoint exemplifies data virtualization by combining data from the **mocked** Product Catalog System and the **mocked** Inventory Management System into a single, unified response. These systems are simulated using hardcoded data to mimic real backend systems. If you'd like to explore another mock API use case, please check out the [RAW Mock API Template](https://github.com/raw-labs/raw-mock-api).

### Features

- **Data Virtualization**: Demonstrates the ability to combine data from multiple backend systems in a single query.
- **Mock Data Integration**: Provides hardcoded data to simulate multiple data sources without the need for actual databases.
- **Dynamic Querying**: Supports optional query parameters to filter and retrieve specific data.
- **Template Structure**: Offers a foundation to build and customize your own data virtualization APIs.

Certainly! Here's the updated **Getting Started** section that explains the two ways to deploy and use the API:

---

## Getting Started

You have two options to deploy and explore the Data Virtualization API Template:

### Option 1: Deploy via the "Get Started" Button

1. **Deploy the API:**
   - Visit the [Data Virtualization Starter Template](https://www.raw-labs.com/templates/data-virtualization-starter).
   - Click the **"Get Started"** button to deploy the template.
   - If you don’t have a RAW account, you’ll be prompted to create one for free. Deployment and account setup are seamless—just one click away!

2. **Explore the API:**
   - Access your API immediately inside the RAW application.
   - View endpoint details and invoke them directly to see how they function.

3. **Customize as Needed:**
   - Modify the API to suit your requirements.
   - Once you’re satisfied, re-publish the changes to make your new API available instantly.

### Option 2: Import the GitHub Repository (Business Plan Required)

1. **Prerequisites:**
   - A RAW account with a **Business Plan** subscription to access the GitHub integration feature.

2. **Import the Repository:**
   - Follow the instructions in the [RAW GitHub Integration Documentation](https://docs.raw-labs.com/docs/github/) to link your GitHub account.
   - Import this GitHub repository into your RAW workspace.

3. **Explore and Customize:**
   - Review the SQL files and endpoints in your RAW workspace.
   - Make any desired modifications to the API.

---

## Domain Entities

The template focuses on two backend systems with the following entities:

- **Product Catalog System**:
  - **Product**: Items available for sale, including details like name, description, and price.

- **Inventory Management System**:
  - **Warehouse**: Storage locations for products.
  - **Inventory**: Stock levels of products in each warehouse.

The entity relationships are the following:

- **Product to Inventory**: One-to-many relationship (a product can be stored in multiple warehouses).
- **Warehouse to Inventory**: One-to-many relationship (a warehouse can store multiple products).

---

**Note**: Since the data is mocked using hardcoded values, the relationships are simulated within the SQL queries.

## Endpoint Overview

### 1. GET `/catalog/products`

- **Description**: Returns a list of products with their names, descriptions, and prices. Supports an optional `product_id` parameter to filter by product.
- **Query Parameters**:
  - `product_id` (integer, optional): The ID of the product.

### 2. GET `/inventory/warehouses`

- **Description**: Returns generic information about warehouses, such as location and manager name. Supports an optional `warehouse_id` parameter to filter by warehouse.
- **Query Parameters**:
  - `warehouse_id` (integer, optional): The ID of the warehouse.

### 3. GET `/inventory/stock` **(Main Endpoint)**

- **Description**: **This endpoint demonstrates data virtualization by combining data from multiple backend systems into a single query.** It returns the stock levels of products in each warehouse, integrating data from both the Product Catalog and Inventory Management systems. Supports optional `product_id` and `warehouse_id` parameters to filter the results.
- **Query Parameters**:
  - `warehouse_id` (integer, optional): The ID of the warehouse.
  - `product_id` (integer, optional): The ID of the product.

## Query Structure

### Basic Structure of SQL Files

- **Parameters**: Defined at the top of each file using comments/annotations in the RAW format.
- **Mock Data**: Created using the `VALUES` clause to simulate tables from different data sources.
- **Variable Injection**: Parameters are injected into the SQL queries using `:<variable_name>`.
- **Conditional Logic**: SQL `WHERE` clauses use parameters to filter data.
- **Proper Table Aliases**: Meaningful aliases are used to enhance readability and maintainability.

### Using Variables

Variables are used within the SQL queries to filter data based on the provided parameters.

**Example Usage in `/inventory/stock`**:

```sql
WHERE (:warehouse_id IS NULL OR Inventory.WarehouseID = :warehouse_id)
  AND (:product_id IS NULL OR Inventory.ProductID = :product_id);
```

- If a parameter is `NULL`, the condition is ignored, and all relevant records are returned.
- If a parameter has a value, the query filters records based on that value.

## Customization

This API template is designed to be easily customizable:

- **Modify SQL Queries**: Adjust the hardcoded data in the `VALUES` clause to fit your use case.
- **Add New Endpoints**: Create new SQL files and define corresponding endpoints in the OpenAPI specification.
- **Change Parameters**: Add or modify parameters to support additional filters or data retrieval options.
- **Integrate with Real Backend Systems**: Replace the mock data with queries against actual backend systems, if needed.

---

## Example SQL Queries

### 1. GET `/catalog/products`

**Description:**

This SQL query retrieves a list of products with their names, descriptions, and prices from the simulated Product Catalog System. It supports an optional `product_id` parameter to filter the results.

**SQL Query:**

```sql
-- @param product_id the ID of the product
-- @type product_id integer
-- @default product_id null

-- @return product information

SELECT ProductID, Name, Description, Price
FROM (
    VALUES
        (1, 'Laptop', 'High-end gaming laptop', 1500.00),
        (2, 'Smartphone', 'Latest model smartphone', 800.00),
        (3, 'Headphones', 'Noise-cancelling headphones', 200.00)
) AS Products (ProductID, Name, Description, Price)
WHERE :product_id IS NULL OR Products.ProductID = :product_id;
```

---

### 2. GET `/inventory/warehouses`

**Description:**

This SQL query retrieves generic information about warehouses from the simulated Inventory Management System. It supports an optional `warehouse_id` parameter to filter the results.

**SQL Query:**

```sql
-- @param warehouse_id the ID of the warehouse
-- @type warehouse_id integer
-- @default warehouse_id null

-- @return warehouse information

SELECT WarehouseID, WarehouseLocation, ManagerName
FROM (
    VALUES
        (1, 'Warehouse A', 'John Doe'),
        (2, 'Warehouse B', 'Jane Smith')
) AS Warehouses (WarehouseID, WarehouseLocation, ManagerName)
WHERE :warehouse_id IS NULL OR Warehouses.WarehouseID = :warehouse_id;
```

---

### 3. GET `/inventory/stock` **(Main Endpoint)**

**Description:**

This SQL query demonstrates **data virtualization** by combining data from the Product Catalog System and the Inventory Management System into a single query. It retrieves the stock levels of products in each warehouse, integrating data from multiple backend systems. It supports optional `warehouse_id` and `product_id` parameters to filter the results.

**SQL Query:**

```sql
-- @param warehouse_id the ID of the warehouse
-- @type warehouse_id integer
-- @default warehouse_id null

-- @param product_id the ID of the product
-- @type product_id integer
-- @default product_id null

-- @return stock levels of products in each warehouse

SELECT Inventory.WarehouseID, Warehouses.WarehouseLocation, Inventory.ProductID, Products.Name AS ProductName, Inventory.StockLevel
FROM (
    VALUES
        (1, 1, 50),   -- WarehouseID, ProductID, StockLevel
        (1, 2, 100),
        (2, 1, 30),
        (2, 3, 200)
) AS Inventory (WarehouseID, ProductID, StockLevel)
INNER JOIN (
    VALUES
        (1, 'Warehouse A', 'John Doe'),
        (2, 'Warehouse B', 'Jane Smith')
) AS Warehouses (WarehouseID, WarehouseLocation, ManagerName)
    ON Inventory.WarehouseID = Warehouses.WarehouseID
INNER JOIN (
    VALUES
        (1, 'Laptop', 'High-end gaming laptop', 1500.00),
        (2, 'Smartphone', 'Latest model smartphone', 800.00),
        (3, 'Headphones', 'Noise-cancelling headphones', 200.00)
) AS Products (ProductID, Name, Description, Price)
    ON Inventory.ProductID = Products.ProductID
WHERE (:warehouse_id IS NULL OR Inventory.WarehouseID = :warehouse_id)
  AND (:product_id IS NULL OR Inventory.ProductID = :product_id);
```

---

**Note:** The top-level comments/annotations describe the input parameters, their types, and default values. Proper table aliases (`Inventory`, `Warehouses`, `Products`) are used for clarity and maintainability.

---

## Support and Troubleshooting

- **Documentation**:
  - Refer to the [RAW Documentation](https://docs.raw-labs.com/docs/) for detailed guides.
    - [Using Data Sources](https://docs.raw-labs.com/docs/sql/data-sources/overview)
    - [Publishing APIs](https://docs.raw-labs.com/docs/publishing-api/overview)
  - **Note**: Since this template uses mock data, connections to external data sources are not required.

- **Community Forum**:
  - Join the discussion on our [Community Forum](https://www.raw-labs.com/community).

- **Contact Support**:
  - Email us at [support@raw-labs.com](mailto:support@raw-labs.com) for assistance.

## License

This project is licensed under the **Apache License 2.0**. See the [LICENSE](LICENSE) file for details.

## Acknowledgements

- **Contributors**: Thanks to all our contributors for their efforts.
- **RAW Platform**: This template demonstrates the capabilities of the RAW platform in creating data virtualization APIs that combine data from multiple backend systems.

## Contact

- **Email**: [support@raw-labs.com](mailto:support@raw-labs.com)
- **Website**: [https://raw-labs.com](https://raw-labs.com)
- **Twitter**: [@RAWLabs](https://twitter.com/raw_labs)
- **Community Forum**: [Forum](https://www.raw-labs.com/community)

---

## Additional Resources

- **RAW Documentation**: Comprehensive guides and references are available at [docs.raw-labs.com](https://docs.raw-labs.com/).
- **Publishing APIs**: Learn how to publish your SQL queries as APIs [here](https://docs.raw-labs.com/docs/publishing-api/overview).
- **SQL Language**: Explore RAW's SQL language for data manipulation [here](https://docs.raw-labs.com/sql/overview).
- **SNAPI Language**: Explore RAW's custom language for data manipulation [here](https://docs.raw-labs.com/snapi/overview).
