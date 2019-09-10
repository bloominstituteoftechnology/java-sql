# java-sql

A student that completes this project shows that they can:
* Query data from a single table
* Query data from multiple tables
* Create a new datadaase using PostgreSQL

# Introduction

Working with SQL

# Instructions

ReImport the Northwind database into PostgreSQL using pgAdmin. This is the same data we used during the guided project.

clone https://github.com/pthom/northwind_psql.git

## pgAdmin

* Right Click Databases
  * Create
    * type in northwind2

* Tools -> Query Tool
  * Open file northwind.sql (from cloned repo)
  * Execute

* Look under
  * northwind2 -> Schemas -> public -> tables

* Clear query windows

Answer the following data queries. Keep track of the SQL you write by pasting it into this document under its appropriate header below. You will be submitting that through the regular fork, change, pull process.


### find all customers that live in London. Returns 6 records.
> This can be done with SELECT and WHERE clauses
```
select contact_title, contact_name, country, city
from customers
where city = 'London'
```


### find all customers with postal code 1010. Returns 3 customers.
> This can be done with SELECT and WHERE clauses
```
select contact_title, contact_name, country, city
from customers
where postal_code = '1010'
```


### find the phone number for the supplier with the id 11. Should be (010) 9984510.
> This can be done with SELECT and WHERE clauses

```
select company_name, phone
from suppliers
where supplier_id = '11'
```

### list orders descending by the order date. The order with date 1998-05-06 should be at the top.
> This can be done with SELECT, WHERE, and ORDER BY clauses
```
select customer_id, ship_name, order_date
from orders
order by order_date desc
```


### find all suppliers who have names longer than 20 characters. You can use `length(company_name)` to get the length of the name. Returns 11 records.
> This can be done with SELECT and WHERE clauses
```
select supplier_id, company_name, city
from suppliers
where length(company_name) > 20
```


### find all customers that include the word 'MARKET' in the contact title. Should return 19 records.
> This can be done with SELECT and a WHERE clause using the LIKE keyword

> Don't forget the wildcard '%' symbols at the beginning and end of your substring to denote it can appear anywhere in the string in question

> Remember to convert your contact title to all upper case for case insenstive comparing so upper(contact_title)
```
select *
from customers
where upper(contact_title) like '%MARKET%'
```

### add a customer record for   
* customer id is 'SHIRE'
* company name is 'The Shire'
* contact name is 'Bilbo Baggins'
* the address is '1 Hobbit-Hole'
* ths city is 'Bag End'
* the postal code is '111'
* the country is 'Middle Earth'
> This can be done with the INSERT INTO clause
```
insert into customers(customer_id, company_name, contact_name, address, city, postal_code, country)
values('SHIRE', 'The Shire', 'Bilbo Baggins', '1 Hobbit-Hole', 'Bag End', '111', 'Middle Earth')

```

### update _Bilbo Baggins_ record so that the postal code changes to _"11122"_.
> This can be done with UPDATE and WHERE clauses
```
update customers
set postal_code = '11122'
where customer_id = 'SHIRE'
```

### list orders grouped by customer showing the number of orders per customer. _Rattlesnake Canyon Grocery_ should have 18 orders.
> This can be done with SELECT, COUNT, JOIN and GROUP BY clauses. Your count should focus on a field in the Orders table, not the Customer table

> There is more information about the COUNT clause on [W3 Schools](https://www.w3schools.com/sql/sql_count_avg_sum.asp)
```
select count(o.order_id), c.contact_name, c.company_name
from orders o join customers c 
on o.customer_id = c.customer_id
group by c.contact_name, c.company_name
order by count(o.order_id)
```

### list customers names and the number of orders per customer. Sort the list by number of orders in descending order. _Save-a-lot Markets should be at the top with 31 orders followed by _Ernst Handle_ with 30 orders. Last should be _Centro comercial Moctezuma_ with 1 order.
> This can be done by adding an ORDER BY clause to the previous answer
```
select count(o.order_id), c.contact_name, c.company_name
from orders o join customers c 
on o.customer_id = c.customer_id
group by c.contact_name, c.company_name
order by count(o.order_id) desc
```

### list orders grouped by customer's city showing number of orders per city. Returns 69 Records with _Aachen_ showing 6 orders and _Albuquerque_ showing 18 orders.
> This is very similar to the previous two queries, however, it focuses on the City rather than the CustomerName

```
select count(o.order_id), c.city
from customers c join orders o
on o.customer_id = c.customer_id
group by c.city
order by c.city
```

## Data Normalization

Note: This step does not use PostgreSQL!

Take the following data and normalize it into a 3NF database.

| Person Name | Pet Name | Pet Type | Pet Name 2 | Pet Type 2 | Pet Name 3 | Pet Type 3 | Fenced Yard | City Dweller |
|-------------|----------|----------|------------|------------|------------|------------|-------------|--------------|
| Jane        | Ellie    | Dog      | Tiger      | Cat        | Toby       | Turtle     | No          | Yes          |
| Bob         | Joe      | Horse    |            |            |            |            | No          | No           |
| Sam         | Ginger   | Dog      | Miss Kitty | Cat        | Bubble     | Fish       | Yes         | No           |




| Person Name |Fenced Yard | City Dweller | Person Id    |
|-------------|------------|--------------|--------------|
| Jane        |No          | Yes          | 1            |
| Bob         |No          | No           | 2            |
| Sam         |Yes         | No           | 3            |


| Person Id   | Pet Id   | Pet Name | Pet Type   | 
|-------------|----------|----------|------------|
| 1           | 1        | Ellie    | Dog        |
| 1           | 2        | Tiger    | Cat        |
| 1           | 3        | Toby     | Turtle     |
| 2           | 4        | Joe      |Horse       | 
| 3           | 5        | Ginger   | Dog        |
| 3           | 6        |Miss Kitty| Cat        |
| 3           | 7        | Bubble   | Fish       |













---
## Stretch Goals

### delete all customers that have no orders. Should delete 17 (or 18 if you haven't deleted the record added) records.
> This is done with a DELETE query

> In the WHERE clause, you can provide another list with an IN keyword this list can be the result of another SELECT query. Write a query to return a list of CustomerIDs that meet the criteria above. Pass that to the IN keyword of the WHERE clause as the list of IDs to be deleted
 
> Use a LEFT JOIN to join the Orders table onto the Customers table and check for a NULL value in the OrderID column

## Create Database and Table

### Keep track of the code you write and paste at the end of this document

- use pgAdmin to create a database, naming it `budget`.
- add an `accounts` table with the following _schema_:

  - `id`, numeric value with no decimal places that should autoincrement.
  - `name`, string, add whatever is necessary to make searching by name faster.
  - `budget` numeric value.

- constraints
  - the `id` should be the primary key for the table.
  - account `name` should be unique.
  - account `budget` is required.
