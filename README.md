# dbt test lab - jaffle shop

## Enter interative shell and start psql
From local machine — `docker exec -it ad143f687e67 /bin/bash`
From container — `psql -d raw -U postgres -W`

## Create analytics db and raw tables
From local machine —
`cat sql/create_analytics_db.sql | docker exec -i ad143f687e67  psql -U postgres -d raw`
`cat sql/create_tables.sql | docker exec -i ad143f687e67  psql -U postgres -d raw`

## Load data
From local machine — `docker cp ../data/  ad143f687e67:/usr/share/`

From container —
`COPY raw.jaffle_shop.customers (ID,FIRST_NAME,LAST_NAME) FROM '/usr/share/data/jaffle_shop_customers.csv' DELIMITER ',' CSV HEADER;`
`COPY raw.jaffle_shop.orders (ID,USER_ID,ORDER_DATE,STATUS) FROM '/usr/share/data/jaffle_shop_orders.csv' DELIMITER ',' CSV HEADER;`
`COPY raw.stripe.payment (ID,ORDERID,PAYMENTMETHOD,STATUS,AMOUNT,CREATED) FROM '/usr/share/data/stripe_payments.csv' DELIMITER ',' CSV HEADER;`cl

## dbt init
In root directory of project - `dbt init`

# psql cheatsheet
```
Connect to a database: psql -U username -d dbname
Exit psql: \q
List all databases: \l or \list
Connect to another database: \c dbname
List tables in current schema: \dt
List tables in all schemas: \dt *.*
Show table definition (including indexes, constraints, triggers): \d tablename
Show more detailed table definition (including physical disk size): \d+ tablename
List all psql meta-commands: \?
Get help on a specific SQL command: \h command_name (e.g., \h CREATE TABLE)
```