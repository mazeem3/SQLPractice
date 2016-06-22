1) How many users are there?
    sqlite> SELECT COUNT(id) FROM users

      50


2) What are the 5 most expensive items?
    sqlite> select * from items
       ...> order by price DESC LIMIT 5;
    id|title|category|description|price
    25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984
    83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
    100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
    40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
    60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341


3) What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
    sqlite> select title, price from items
       ...> where category = "Books" order by price ASC;
    title|price
    Ergonomic Granite Chair|1496

    The cheapest remains the same however the category contains method returns more items with the category books.

4) Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
    select users.first_name, users.last_name, addresses.street, addresses.city, addresses.state
       ...>     from users
       ...>     join addresses on users.id = addresses.user_id
       ...>     where addresses.street = "6439 Zetta Hills";
    Corrine|Little|6439 Zetta Hills|Willmouth|WY
    sqlite> select users.first_name, users.last_name, addresses.street, addresses.city, addresses.state
       ...>     from users
       ...>     join addresses on users.id = addresses.user_id
       ...>     where users.last_name = "Little";
    Corrine|Little|6439 Zetta Hills|Willmouth|WY
    Corrine|Little|54369 Wolff Forges|Lake Bryon|CA


5) Correct Virginie Mitchell's address to "New York, NY, 10108".
    sqlite> select users. id, addresses.id, users.first_name, users.last_name, addresses.street, addresses.city, addresses.state, addresses.zip, addresses.user_id
       ...>
       ...>      from users
       ...>      join addresses on users.id = addresses.user_id
       ...>      where users.first_name = "Virginie" AND users.last_name = "Mitchell";
    39|41|Virginie|Mitchell|12263 Jake Crossing|Roxanehaven|NY|34705|39
    39|42|Virginie|Mitchell|83221 Mafalda Canyon|Bahringerland|WY|24028|39
    sqlite> update addresses set city = "New York", state = "NY", zip = 10108
       ...> where user_id = 39;
    sqlite> select * from addresses where zip = 10108;
    41|39|12263 Jake Crossing|New York|NY|10108
    42|39|83221 Mafalda Canyon|New York|NY|10108
    sqlite> select users. id, addresses.id, users.first_name, users.last_name, addresses.street, addresses.city, addresses.state, addresses.zip, addresses.user_id
       ...>
       ...>      from users
       ...>      join addresses on users.id = addresses.user_id
       ...>      where users.first_name = "Virginie" AND users.last_name = "Mitchell";
    39|41|Virginie|Mitchell|12263 Jake Crossing|New York|NY|10108|39
    39|42|Virginie|Mitchell|83221 Mafalda Canyon|New York|NY|10108|39


6) How much would it cost to buy one of each tool?
    select sum (price) from items where category = "Tools";
    7383

7) How many total items did we sell?
    select sum(quantity) from orders;
    2125

8) How much was spent on books?
    sqlite> select sum(items.price * orders.quantity) from items
       ...> inner join orders on items.id = orders.item_id
       ...> where items.category = "Books";
    420566


9) Simulate buying an item by inserting a User for yourself and an Order for that User.
    sqlite> insert into users (id, first_name, last_name, email)
       ...> values (51, "Maaz", "Azeem", "myemail@address.com")
       ...> ;
    sqlite> select * from users where last_name = "Azeem";
    id|first_name|last_name|email
    51|Maaz|Azeem|myemail@address.com
    sqlite> insert into orders(user_id, item_id, quantity)
       ...> values (51, 3, 100);
    sqlite> select * from orders where user_id = 51;
    id|user_id|item_id|quantity|created_at
    378|51|3|100|
