# Amazon Book ETL Pipeline <br />

This ETL pipeline scrapes data form Amazon.com for software engineering books daily (Extraction). Then, the data is cleaned (Transform) and loaded onto pgAdmin (Load). 



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
