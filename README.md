# RAW Database Virtualization API Template

## Introduction

This repository provides a **Database Virtualization API Template** using the RAW platform. It demonstrates how to create API endpoints that combine data from multiple **databases** into a single response using SQL queries. The central point of this template is to showcase RAW's ability to perform data virtualization by integrating data from various databases in a single query. This approach reflects real-world scenarios where data is stored across multiple databases, and there is a need to provide unified access to this data.

### How It Works

The RAW platform allows you to create APIs by writing SQL queries that can access and combine data from various data sources, including databases. In this template, we use two databases named `catalog` and `inventory`. The endpoints accept optional query parameters.

**Key Point:** The `/inventory/stock` endpoint exemplifies data virtualization by combining data from the **mocked** `catalog` and `inventory` databases into a single, unified response. These databases are simulated using hardcoded data to mimic real backend systems.

### Features

- **Data Virtualization**: Demonstrates the ability to combine data from multiple databases in a single query.
- **Mock Data Integration**: Provides hardcoded data to simulate multiple data sources without the need for actual databases.
- **Query Parameters**: Supports query parameters to filter and retrieve specific data.
- **Template Structure**: Offers a foundation to build and customize your own data virtualization APIs.

## Getting Started

1. **Deploy the API:**
   - Visit the [Database Virtualization Starter](https://www.raw-labs.com/templates/database-virtualization-api-starter) page.
   - Click the **"Get Started"** button to deploy the template.
   - If you don’t have a RAW account, you’ll be prompted to create one for free. Deployment and account setup are seamless—just one click away!

2. **Explore the API:**
   - Access your API immediately inside the RAW application.
   - View endpoint details and invoke them directly to see how they function.

3. **Customize as Needed:**
   - Modify the API to suit your requirements.
   - Once you’re satisfied, re-publish the changes to make your new API available instantly.

## Overview

The template focuses on two databases with the following entities:

- **Catalog Database**:
  - **Product**: Items available for sale, including details like name, description, and price.

- **Inventory Database**:
  - **Warehouse**: Storage locations for products.
  - **Inventory**: Stock levels of products in each warehouse.

The entity relationships are the following:

- **Product to Inventory**: One-to-many relationship (a product can be stored in multiple warehouses).
- **Warehouse to Inventory**: One-to-many relationship (a warehouse can store multiple products).

## Endpoints

### 1. GET `/catalog/products`

- **Description**: Returns a list of products with their names, descriptions, and prices. Supports an optional `product_id` parameter to filter by product.
- **Query Parameters**:
  - `product_id` (integer, optional): The ID of the product.
- Source code at [/catalog/products.sql](/catalog/products.sql) and endpoint definition at [/catalog/products.yml](/catalog/products.yml).

### 2. GET `/inventory/warehouses`

- **Description**: Returns generic information about warehouses, such as location and manager name. Supports an optional `warehouse_id` parameter to filter by warehouse.
- **Query Parameters**:
  - `warehouse_id` (integer, optional): The ID of the warehouse.
- Source code at [/inventory/warehouses.sql](/inventory/warehouses.sql) and endpoint definition at [/inventory/warehouses.yml](/inventory/warehouses.yml).

### 3. GET `/inventory/stock` **(Main Endpoint)**

- **Description**: **This endpoint demonstrates data virtualization by combining data from multiple databases into a single query.** It returns the stock levels of products in each warehouse, integrating data from both the `catalog` and `inventory` databases. Supports optional `product_id` and `warehouse_id` parameters to filter the results.
- **Query Parameters**:
  - `warehouse_id` (integer, optional): The ID of the warehouse.
  - `product_id` (integer, optional): The ID of the product.
- Source code at [/inventory/stock.sql](/inventory/stock.sql) and endpoint definition at [/inventory/stock.yml](/inventory/stock.yml).

## Short Intro to RAW APIs

In RAW, APIs consist of two parts: a YAML file for endpoint configuration and a SQL file for the query logic. The YAML file path defines the API’s endpoint. For example, /inventory/stock.yaml corresponds to the API path /inventory/stock.

SQL queries can include dynamic parameters using the `:<variable_name>` syntax. For instance:
```sql
WHERE CustomerID = :customer_id
```
Here, `:customer_id` becomes a query parameter, accessible via the API as `?customer_id=<value>`.

To document parameters, enforce types or default values, add metadata at the top of the SQL file as in:
```sql
-- @param customer_id the ID of the customer  
-- @type customer_id integer  
-- @default customer_id null
```

## Next Steps

Visit the [Database Virtualization Starter](https://www.raw-labs.com/templates/database-virtualization-api-starter) page, deploy this template and get started using RAW.

When you create your RAW account, you will be able to view and run these endpoints in the RAW catalog, as well as quickly modify these endpoints or create new ones in the RAW workspace, in our easy-to-use web IDE.

## License

This project is licensed under the **Apache License 2.0**. See the [LICENSE](LICENSE) file for details.

## Contact

- **Website**: [www.raw-labs.com](https://www.raw-labs.com)
- **Twitter**: [@raw_labs](https://twitter.com/raw_labs)
- **Contact Support**: Email us at [support@raw-labs.com](mailto:support@raw-labs.com) for assistance
- **Community Forum**: Join the discussion on our [Community Forum](https://www.raw-labs.com/community)

For more information, visit our [documentation](https://docs.raw-labs.com).
