# dbt test lab - jaffle shop

## Bring up postgres db
From local machine - `docker compose up -d`

## Enter interative shell and start psql
From local machine — `docker exec -it {container id} /bin/bash`
From container — `psql -d analytics_dbt -U postgres -W`

## Create analytics db and raw tables
From local machine —
`cat sql/create_analytics_db.sql | docker exec -i {container id}  psql -U postgres -d analytics_dbt`
`cat sql/create_tables.sql | docker exec -i {container id}  psql -U postgres -d analytics_dbt`

## Load data
From local machine — `docker cp data/  {container id}:/usr/share/`

From container, in `psql` —
* `COPY analytics_dbt.jaffle_shop.customers (ID,FIRST_NAME,LAST_NAME) FROM '/usr/share/data/jaffle_shop_customers.csv' DELIMITER ',' CSV HEADER;`
* `COPY analytics_dbt.jaffle_shop.orders (ID,USER_ID,ORDER_DATE,STATUS) FROM '/usr/share/data/jaffle_shop_orders.csv' DELIMITER ',' CSV HEADER;`
* `COPY analytics_dbt.stripe.payment (ID,ORDERID,PAYMENTMETHOD,STATUS,AMOUNT,CREATED) FROM '/usr/share/data/stripe_payments.csv' DELIMITER ',' CSV HEADER;`

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