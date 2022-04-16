## Database Tuning using Natural Language Processing

[![Made With python 3.8.2](https://img.shields.io/badge/Made%20with-Python%203.8.2-brightgreen)](https://www.python.org/downloads/release/python-382/)
[![Pytorch](https://img.shields.io/badge/Made%20with-Pytorch-green.svg)](https://pytorc.org/)

### Jupyter notebook for configuration recommendations

The following notebooks can be executed in Colab to obtain results of NLP pipeline

* NLP_Based_DB_Tuning_Roberta_MySQL.ipynb
* NLP_Based_DB_Tuning_Roberta_postgres.ipynb

### Configuation Files

Update parameters of configurations files as per the recommendations. 

These configuration files are available in the repo at config/mysql and config/postgresq respectively


### TPC-H Benchmark for postgresql

#### Start and Stop postgresql services

> sudo systemctl stop postgresql.service

> sudo systemctl start postgresql.service

> sudo systemctl status postgresql.service


#### Ceate user and database for the benchmark

> sudo -u postgres createuser tpch

> sudo -u postgres createdb tpchdb

> sudo -u postgres psql  << PSQL

> ALTER USER tpch WITH ENCRYPTED PASSWORD ‘password’123;

> GRANT ALL PRIVILEGES ON DATABASE tpchdb TO tpch;

> \l

> \q

> PSQL

> sudo -i -u postgres
> psql
> ALTER USER postgres WITH ENCRYPTED PASSWORD 'test123';


#### Prepare, Load and Query phases for tpch benchmark

> git clone https://github.com/ksasi/tpch-pgsql.git

> cd tpch-pgsql
> 
> python3 tpch_pgsql.py prepare -U postgres -W test123 -d tpchdb
> 
> python3 tpch_pgsql.py load -U postgres -W test123 -d tpchdb
> 
> python3 tpch_pgsql.py query -U postgres -W test123 -d tpchdb

### postgresql tuner

> sudo apt install libdbd-pg-perl libdbi-perl perl-modules

> wget -O postgresqltuner.pl postgresqltuner.pl

> chmod +x postgresqltuner.pl

> ./postgresqltuner.pl --host=localhost --database=tpchdb --user=postgres --password=test123



### TPC-H Benchmark for mysql

#### Start and Stop mysql services

> sudo systemctl stop mysql.service

> sudo systemctl start mysql.service

> sudo systemctl status mysql.service

> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
 

#### Ceate user and database for the benchmark

> mysql -u root -p

> CREATE USER 'tpch'@'%' IDENTIFIED BY 'Test1234$';

> CREATE DATABASE tpch;

> GRANT ALL ON tpch.* to 'tpch'@'%'; 


#### Prepare mysql for tpch benchmark

> git clone https://github.com/acshulyak/mysql_tpch.git

Replace the files in mysql_tpch/dbgen directly with the files in this repo (tpch/mysql) i.e. dss.ri, makefile and tpcd.h

Change directory location to mysql_tpch/dbgen

> mysql -u root -p

> USE tpch;

> \. ./dbgen/dss.ddl

> make
 
> ./dbgen -s 1
 

> mysql -u root -p

> LOAD DATA LOCAL INFILE 'customer.tbl' INTO TABLE CUSTOMER FIELDS TERMINATED BY '|';
> 
> LOAD DATA LOCAL INFILE 'orders.tbl' INTO TABLE ORDERS FIELDS TERMINATED BY '|';
> 
> LOAD DATA LOCAL INFILE 'lineitem.tbl' INTO TABLE LINEITEM  FIELDS TERMINATED BY '|';
> 
> LOAD DATA LOCAL INFILE 'nation.tbl' INTO TABLE NATION   FIELDS TERMINATED BY '|';
> 
> LOAD DATA LOCAL INFILE 'partsupp.tbl' INTO TABLE PARTSUPP FIELDS TERMINATED BY '|';
> 
> LOAD DATA LOCAL INFILE 'part.tbl' INTO TABLE PART  FIELDS TERMINATED BY '|';
> 
> LOAD DATA LOCAL INFILE 'region.tbl' INTO TABLE REGION FIELDS TERMINATED BY '|';
> 
> LOAD DATA LOCAL INFILE 'supplier.tbl' INTO TABLE SUPPLIER  FIELDS TERMINATED BY '|';
> 
> \. dss.ri

#### run tpch benchmark for mysql

> git clone https://github.com/catarinaribeir0/queries-tpch-dbgen-mysql.git
> 
Copy the file run_all.sh from this repo (tpch/mysql) to queries-tpch-dbgen-mysql location

> time sh run_all.sh
 

### mysql tuner 

> git clone https://github.com/major/MySQLTuner-perl
> 
> perl mysqltuner.pl --verbose --outputfile /home/kotti_1/SDE_Project/mysql/result1_mysqltuner.txt
> 
> perl mysqltuner.pl --buffers --dbstat --idxstat --sysstat --pfstat --tbstat --outputfile /home/kotti_1/SDE_Project/mysql/result2_mysqltuner.txt




