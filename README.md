# Amazon Book ETL Pipeline <br />

This ETL pipeline performs daily data scraping from Amazon.com to gather information on software engineering books. The data is then fetched, cleaned, and loaded into a PostgreSQL database, following a Directed Acyclic Graph (DAG) workflow.



## <a name="technologies"></a> Technologies
* Python
* Apache Airflow
* pgAdmin PostgreSQL
* Docker
* Pandas
* DAG
* Beautifulsoup







### Graph of Directed Acyclic Graph (DAG) Being Executed in Apache Airflow <br />
![DAG](/static/images/DAG.png "DAG") <br/>



### Query to Show Data Table Has Been Populated in Database <br />
![Table in Database](/static/images/all_books.png "Table in Database") <br />



### Query of Top Ten Books by Rating <br />
![Top Ten Books by Rating](/static/images/high_rated.png "Top Ten Books by Rating") <br/>
