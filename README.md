## Music in Brazil 

The chinook sample database is a good database for practicing with SQL, especially SQLite. (source:https://www.sqlitetutorial.net/sqlite-sample-database/)

This is the following schema:


![schema_chinook](https://user-images.githubusercontent.com/82055743/166265207-312287ea-22be-4041-8bf2-9b2e72029e6e.png)

Queries:

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

- Select the selling amount of 'Mais do mesmo' album

```
SELECT t.name, sum(inv_item.quantity) AS "Total quantity", (inv_item.unitprice * sum(inv_item.quantity)) as "Total price"
FROM tracks t
LEFT JOIN invoice_items inv_item
ON t.trackid = inv_item.trackid
LEFT JOIN albums a
ON t.albumid = a.albumid
WHERE a.title = "Mais Do Mesmo"
GROUP BY t.name;
```

-
