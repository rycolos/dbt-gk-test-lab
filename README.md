## Enter interative shell and start psql
From local machine — docker exec -it ad143f687e67 /bin/bash
From container — psql -d raw -U postgres -W

## Create analytics db and raw tables
From local machine —
cat sql/create_analytics_db.sql | docker exec -i ad143f687e67  psql -U postgres -d raw
cat sql/create_tables.sql | docker exec -i ad143f687e67  psql -U postgres -d raw

## Load data
From local machine — docker cp ../data/  ad143f687e67:/usr/share/

From container —
COPY raw.jaffle_shop.customers (ID,FIRST_NAME,LAST_NAME) FROM '/usr/share/data/jaffle_shop_customers.csv' DELIMITER ',' CSV HEADER;
COPY raw.jaffle_shop.orders (ID,USER_ID,ORDER_DATE,STATUS) FROM '/usr/share/data/jaffle_shop_orders.csv' DELIMITER ',' CSV HEADER;
COPY raw.stripe.payment (ID,ORDERID,PAYMENTMETHOD,STATUS,AMOUNT,CREATED) FROM '/usr/share/data/stripe_payments.csv' DELIMITER ',' CSV HEADER;

## dbt init
In root directory of project - `dbt init`