# PostgreSQL Replication Load Balancing with Pgpool-II

Database replication involves copying, transferring, or integrating data from one database in a server or computer to another, eventually creating a distributed database. The purpose of replication is to 

load balancing is the process of distributing a set of tasks over a set of resources (computing units), with the aim of making their overall processing more efficient.  

Pgpool-II is a middleware that works between PostgreSQL servers and a PostgreSQL database client. It can manage multiple PostgreSQL servers. Using the replication function enables creating a realtime backup on 2 or more physical disks, so that the service can continue without stopping servers in case of a disk failure. 

If a database is replicated, executing a SELECT query on any server will return the same result. Pgpool-II takes an advantage of the replication feature to reduce the load on each PostgreSQL server by distributing SELECT queries among multiple servers, improving system's overall throughput. At best, performance improves proportionally to the number of PostgreSQL servers. Load balance works best in a situation where there are a lot of users executing many queries at the same time.


First pull the repository and run the below command to complete the installation:
!! Be sure that you copy the `.env.example` file to`.env` and change based on your local configuration !!
```
docker-compose up -d
docker-compose exec -it pgpool psql -U <DB_USER> -h localhost -p 5432 -d <POSTGRESQL_DATABASE>

<db_name>=# CREATE TABLE users (id SERIAL PRIMARY KEY, email VARCHAR(100) NOT NULL);
<db_name>=# INSERT INTO users (email) VALUES ('rakib@example.com'), ('chayan@example.com'), ('sumon@example.com');
<db_name>=# SELECT * FROM users;
... repeat couple of times
<db_name>=# SELECT * FROM users;
```
In Master Database:
```
docker-compose logs -f pg-master

docker-compose exec -it pg-master psql -U <DB_USER> -h localhost -p 5432 -d <POSTGRESQL_DATABASE>
```

In Standby Database:
```
docker-compose logs -f pg-standby

docker-compose exec -it pg-standby psql -U <DB_USER> -h localhost -p 5432 -d <POSTGRESQL_DATABASE>
```
