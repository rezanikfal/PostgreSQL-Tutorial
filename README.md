## Create table with auto-generate primary key and foreign key
    CREATE TABLE photos(
      id SERIAL PRIMARY KEY,
      url VARCHAR(200),
      user_id INTEGER REFERENCES users(id)
    );
    
## Insert data into table
    INSERT INTO photos (url, user_id)
    VALUES
    ('http://two.jpg',1),
    ('http://23.jpg',1),
    ('http://2.jpg',4);
    
## Foreign Key Constraints for Deletion
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
    user_id INTEGER REFERENCES users(id) ON DELETE SET NULL
    user_id INTEGER REFERENCES users(id) ON DELETE SET DEFAULT
    user_id INTEGER REFERENCES users(id) ON DELETE RESTRICT (Default)
    
## Query with Join Aggregation   
    select contents, username
    FROM comments
    JOIN users ON users.id = comments.user_id

## Four types of Join  
![Jion1 Preview Shot](./Join1.JPG)

![Jion2 Preview Shot](./Join2.JPG)

## Count aggregate function
It does not count the NULL values. To fix this issue we can use "*":

    SELECT user_id, COUNT(*) from COMMENTS GROUP BY user_id

## SQL commands order 
![SQL Order Preview Shot](./SQLOrder.jpg)

## Filtering by "Having" 
    SELECT photo_id, COUNT(*)
    FROM comments
    WHERE photo_id<3
    GROUP BY photo_id
    HAVING COUNT(*) > 10
    
## Order By
By default it is Ascending (ASC)

    select *
    FROM products
    ORDER BY price, weight DESC;

## LIMIT & OFFSET
OFFSET skips the records and LIMIT generates the exact number of records  
LIMIT does not exist in SQL-Server. There are FETCH & TOP instead  
Second & third most expensive phones:

    SELECT name 
    FROM phones
    ORDER BY price desc
    LIMIT 2
    OFFSET 1;

## UNION
Manufacturer(s) that make phones with price less than 170. Also Manufacturer(s) that make more that 2 phones

    SELECT manufacturer FROM phones WHERE price<170
    UNION
    SELECT manufacturer FROM phones GROUP BY manufacturer HAVING COUNT(*)>2
    
 ## Subqueries  with Where
When the Subqueries  returns a column, we use "IN" to get the data.

    SELECT id FROM orders
        WHERE product_id IN (
      SELECT id FROM products WHERE price/weight <10
    );
    
When the Subqueries  returns a single value
    
    SELECT name, price from phones 
    where price > (select price from phones where name= "S5620 Monte" );
    
## All & SOME operators
      
Price is bigger than all subquery prices for "ALL" and bigger than at least one subquery prices for "SOME".

    SELECT name from phones
    WHERE price > ALL(SELECT price FROM phones WHERE manufacturer = 'Samsung');
    
## SELECT without FROM
      
Select from subqueies only when they generate "ONE" value

    SELECT 
    (SELECT MAX(price) FROM phones) AS max_price,
    (SELECT MIN(price) FROM phones) AS min_price,
    (SELECT AVG(price) FROM phones) AS avg_price

## DISTINCT

    SELECT COUNT(DISTINCT manufacturer)
    from phones;
    
  ## GREATEST, LEAST (not in SQL-Server)
  
  If "2*weight" is less than 30, then return 30 for any product. If not return 2 * weight.
        
    SELECT name, GREATEST(30, 2 * weight)
    FROM products
