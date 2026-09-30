# northwind-date-warehouse
Designing a star schema data warehouse based on the Northwind database and implementing the ETL process using the SSIS tool.

## Overview:

The main purpose of this project is to implement the concept of a data warehouse using dimensional modeling, which involves identifying business processes, dimensions, facts, and granularity.
Develop a star schema design for the data warehouse.
Finally, the project involves loading data into the data warehouse using an SSIS ETL tool.

## Source and database

Northwind is a widely used educational database created by Microsoft, designed to represent a small, fictional company's data, such as its customers, orders, products, and suppliers. It includes tables for employees, customers, orders, order details, products, suppliers, and more. This database serves as a practical and realistic example for demonstrating various database concepts, including data modeling.

## Star Schema for the datawarehouse :

![275851290-f423ab77-5be8-4f26-b6a4-b52a39baf8c2](https://github.com/Arwa0/northwind-date-warehouse/assets/74055031/913f2124-0c9e-49d0-af87-7ede034e41b2)


 A star schema has one fact table containing all the measurements we need about the any order have been placed , and containing all foreign keys from other dimentions. including the key of the bridged table 'ship_info'

## ETL process:

After designing the data warehouse, the implementation of it was carried out on SQL Server, as its code is shown in the file.
Using SSIS, I implemented the ETL process on this sequence :


![image](https://github.com/Arwa0/northwind-date-warehouse/assets/74055031/52d586f0-fe20-4b0f-b326-d935fd1eb001)



There is an example for loading data in ' ship_info ' :


![275880971-df8289ee-72a7-4135-bdae-121fb033e38a](https://github.com/Arwa0/northwind-date-warehouse/assets/74055031/14576a7b-668a-490e-b5d0-1ead03fc7a02)

For Fact Table loading:


The first step involved collecting all the IDs from tables in the database using a Merge Join transformation in the same place. Next, I used a Lookup transformation to obtain the surrogate key for each dimension, applying any necessary transformations as needed. Here is the data flow task for the fact table :
 
![Untitled](https://github.com/Arwa0/northwind-date-warehouse/assets/74055031/045eb39c-3f17-4655-bfc9-2ad37260fbf9)



for more about the transformation done in fact table , you can download the project and see it !
