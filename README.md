## :musical_note:	 Music in Brazil 

### Introduction

In this project I made queries from a music database using SQL.

The chinook sample database is a good database for practicing with SQL, especially SQLite. (source:https://www.sqlitetutorial.net/sqlite-sample-database/)

This is the following schema and the dictionary:

![schema_chinook](https://user-images.githubusercontent.com/82055743/166265207-312287ea-22be-4041-8bf2-9b2e72029e6e.png)

image source:https://www.sqlitetutorial.net/sqlite-sample-database/

![dict_music](https://user-images.githubusercontent.com/82055743/166426668-c19e504d-a05f-4374-86ca-3f546d36e3a4.png)

image source:https://www.sqlitetutorial.net/sqlite-sample-database/





### Queries:


- 3 best selling genres in Brazil

```
SELECT g.name
FROM genres g
INNER JOIN tracks t 
ON g.genreid= t.genreid
INNER JOIN  invoice_items inv_item
ON t.trackid = inv_item.trackid
INNER JOIN  invoices inv
ON inv.invoiceid =inv_item.invoiceid
WHERE inv.billingcountry = "Brazil"
GROUP BY g.name
ORDER BY count(g.genreid) DESC
LIMIT 3;
```

- Selling amount (price and quantity) of 'Mais do mesmo' album

```
SELECT t.name, sum(inv_item.quantity) AS "Total quantity", (inv_item.unitprice * sum(inv_item.quantity)) AS "Total price"
FROM tracks t
LEFT JOIN invoice_items inv_item
ON t.trackid = inv_item.trackid
LEFT JOIN albums a
ON t.albumid = a.albumid
WHERE a.title = "Mais Do Mesmo"
GROUP BY t.name;
```

- Total selling by salesperson in 2012

```
SELECT e.firstname, e.lastname, sum(inv.total) AS "Total amount of selling in 2012"
FROM customers c 
INNER JOIN employees e
ON e.employeeid = c.supportrepid 
INNER JOIN invoices inv
ON inv.customerid = c.customerid
WHERE strftime('%Y', inv.invoicedate) = '2012'
GROUP BY e.employeeid
ORDER BY "Total amount of selling in 2012" DESC;
```

- Total songs, total songs with MPEG media and the Total with AAC media for each genre

```
SELECT g.name as "Gênero", COUNT(t.trackid) AS "Total songs", 
COUNT(CASE WHEN m.name LIKE "%mpeg%" THEN 1 END) AS "Total MPEG media ",
COUNT(CASE WHEN m.name like "%aac%" THEN 1 END) AS "Total AAC media"
FROM genres g
INNER JOIN tracks t
ON t.genreid = g.genreid
INNER JOIN media_types m
ON t.mediatypeid = m.mediatypeid
GROUP BY g.name;
```


