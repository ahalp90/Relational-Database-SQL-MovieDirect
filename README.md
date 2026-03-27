# MovieDirect SQL Views

SQL views for querying and analysing data from a relational movie shipment database. Built as a database project using PostgreSQL.

## Repository Contents

*   `README.md`: This file, providing an overview of the project and setup instructions.
*   `MovieDirect_Data.sql`: The SQL script required to create the database schema (tables) and populate them with the necessary data.
*   `movie_database_views.sql`: The main SQL script containing the `CREATE VIEW` statements.

## Setup and Usage

To run this project and test the views, follow these steps:

1.  **Create a Database**: In your chosen SQL environment (e.g., PostgreSQL, MySQL, SQL Server), create a new, empty database.
2.  **Load Data**: Execute the `MovieDirect_Data.sql` script. This will create the required tables (`Movies`, `Customers`, `Stock`, `Shipments`) and insert the sample data.
3.  **Create Views**: Execute `movie_database_views.sql`. This will create all the views.
4.  **Query the Views**: You can now query the data using the views. For example:
```sql
    SELECT * FROM movie_summary;
    SELECT * FROM binge_watcher;
```

---

## Views Created

#### 1. `movie_summary(movie_title, release_date, media_type, retail_price)`
Provides a simple summary of each movie available in stock, combining information from the `Movies` and `Stock` tables.

#### 2. `old_shipments(first_name, last_name, movie_id, shipment_id, shipment_date)`
Lists all historical shipments that occurred before 1 January 2010, including the name of the customer who received the shipment.

#### 3. `richie(movie_title)`
Finds all movies in the database directed by Ron Howard.

#### 4. `retail_price_hike(movie_id, retail_price, new_price)`
Calculates a potential new retail price for all items in stock. The `new_price` column represents a 25% increase on the current `retail_price`.

#### 5. `profits_from_movie(movie_id, movie_title, total_profit)`
Calculates the total profit generated from movies that have been shipped. Profit is calculated as the sum of retail prices minus the sum of cost prices for all shipped units of a particular movie. Only includes movies with at least one shipment record.

#### 6. `binge_watcher(first_name, last_name)`
Identifies customers who can be considered "binge watchers" by finding those who have had more than one movie shipped to them on the same day.

#### 7. `the_sith(first_name, last_name)`
Lists all customers who have **never** had the movie 'Star Wars: Episode V - The Empire Strikes Back' shipped to them. Uses a `NOT EXISTS` subquery to filter the customer list.

#### 8. `sole_angry_man(first_name, last_name)`
Identifies the customer who had '12 Angry Men' shipped, but **only if** they were the single, sole customer to have ever received that movie. If more than one customer has received the movie, this view returns no results.

---

## Implementation Notes

*   **Output Ordering**: No ordering requirements were specified for this schema, so `ORDER BY` clauses have been omitted.
*   **Rounding**: Financial values in `retail_price_hike` and `profits_from_movie` are returned as precise floating-point numbers. In a production environment, these would likely be rounded to two decimal places.
*   **Distinct Values**: The current implementation preserves duplicates where the underlying data allows. For views like `richie` or `binge_watcher`, this means a movie title or customer name could potentially appear multiple times.
*   **Grouping Logic**: In views requiring aggregation (e.g., `profits_from_movie`), grouping has been performed by the primary key (`movie_id`) in addition to other attributes like `movie_title` to ensure correct and unambiguous aggregation.
