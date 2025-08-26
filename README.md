# Amazon Book ETL Pipeline <br />

This ETL pipeline performs daily data scraping from Amazon.com to gather information on software engineering books. The data is then fetched, cleaned, and loaded into a PostgreSQL database, following a Directed Acyclic Graph (DAG) workflow.



## <a name="technologies"></a> Technologies
* Python
* Apache Airflow (DAG)
* PostgreSQL (pgAdmin)
* Docker
* Pandas
* Beautifulsoup4


## <a name="features"></a> Features


This Apache Airflow DAG implements a comprehensive automated ETL (Extract, Transform, Load) pipeline designed to scrape software engineering book data from Amazon's search results and systematically store it in a PostgreSQL database. The workflow is structured as a directed acyclic graph with three interdependent tasks that execute sequentially: data extraction through web scraping, database table creation, and data insertion. The pipeline is configured with a daily execution schedule (`timedelta(days=1)`) and includes robust error handling mechanisms with automatic retry logic (1 retry attempt with a 5-minute delay) to ensure reliability in production environments.

The data extraction phase employs sophisticated web scraping techniques using Python's requests library combined with BeautifulSoup for HTML parsing. The scraping function (`get_amazon_data_books`) targets Amazon's search results for "software engineering books" and implements several anti-detection strategies including custom HTTP headers that mimic a legitimate macOS Chrome browser, complete with referrer information and security headers. The scraper navigates through multiple pages of results using dynamic URL construction with pagination parameters, systematically extracting four key data points from each book: title, author, price, and rating information. It uses specific CSS selectors to locate HTML elements (`s-result-item` for containers, `a-text-normal` for titles, `a-size-base` for authors, `a-price-whole` for prices, and `a-icon-alt` for ratings), ensuring precise data extraction from Amazon's complex page structure.

The data processing and quality control mechanisms are built into the extraction phase, where the system maintains a `seen_titles` set to prevent duplicate entries and validates that all required fields are present before adding records to the dataset. The scraped data is converted into a pandas DataFrame for efficient manipulation, with duplicate removal performed using the `drop_duplicates` method based on book titles. Data is then serialized and passed between tasks using Airflow's XCom (cross-communication) system, which enables reliable inter-task data transfer within the workflow.

The database operations utilize Airflow's PostgreSQL integration through multiple components: the PostgresOperator for executing raw SQL commands to create the books table with appropriate schema (including auto-incrementing primary key, text fields for title, authors, price, and rating), and PostgresHook for programmatic database interactions during the data insertion phase. The `insert_book_data_into_postgres` function retrieves the scraped data from XCom, establishes a database connection using the pre-configured `books_connection` identifier, and performs batch insertions using parameterized queries to prevent SQL injection attacks. The database schema is designed with a `CREATE TABLE IF NOT EXISTS` approach, ensuring idempotency and allowing the pipeline to run safely multiple times without conflicts.

The technical architecture leverages a comprehensive stack of tools including core Python libraries (datetime for temporal operations), specialized data processing libraries (pandas for DataFrame operations), web scraping tools (requests for HTTP operations and BeautifulSoup for HTML parsing), and Airflow's orchestration capabilities (DAG for workflow definition, PythonOperator for custom Python logic execution, PostgresOperator for database operations). The pipeline's design follows ETL best practices by clearly separating concerns: extraction logic is isolated in dedicated Python functions, transformation occurs within the pandas DataFrame operations, and loading is handled through dedicated database operators. The task dependency chain (`fetch_book_data_task >> create_table_task >> insert_book_data_task`) ensures proper execution order and data flow, while the default arguments configuration provides consistent behavior across all tasks including ownership attribution, scheduling parameters, and failure handling policies.



### Graph of Directed Acyclic Graph (DAG) Being Executed in Apache Airflow <br />
![DAG](/static/images/DAG.png "DAG") <br/>



### Query to Show Data Table Has Been Populated in Database <br />
![Table in Database](/static/images/all_books.png "Table in Database") <br />



### Query of Top Ten Books by Rating <br />
![Top Ten Books by Rating](/static/images/high_rated.png "Top Ten Books by Rating") <br/>
