# otus-pgsql-hw-lesson-7

1.  создайте новый кластер PostgresSQL 14

done

2.  зайдите в созданный кластер под пользователем postgres

        postgres=#
        postgres=#

3.  создайте новую базу данных testdb

CREATE DATABASE testdb;

        postgres=# CREATE DATABASE testdb;
        CREATE DATABASE
        postgres=#

4.  зайдите в созданную базу данных под пользователем postgres

        postgres=# \c testdb;
        You are now connected to database "testdb" as user "postgres".
        testdb=#

5.  создайте новую схему create schema testnm;

        testdb=# create schema testnm;
        CREATE SCHEMA
        testdb=#

6.  создайте новую таблицу t1 с одной колонкой c1 типа integer create table t1(c1 int);

        testdb=# create table t1(c1 int);
        CREATE TABLE
        testdb=#

7.  вставьте строку со значением c1=1 insert into t1 values (1);

        testdb=# insert into t1 values (1);
        INSERT 0 1
        testdb=# select * from t1;
         c1
        ----
          1
        (1 row)
        
        testdb=#


8.  создайте новую роль readonly

                testdb=# CREATE role readonly;
                CREATE ROLE
                testdb=#


9.  дайте новой роли право на подключение к базе данных testdb

                testdb=# grant connect on DATABASE testdb TO readonly;
                GRANT
                testdb=#


10.  дайте новой роли право на использование схемы testnm

                testdb=# grant usage on SCHEMA testnm to readonly;
                GRANT


11.  дайте новой роли право на select для всех таблиц схемы testnm

                testdb=# grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
                GRANT
                testdb=#

12.  создайте пользователя testread с паролем test123

                testdb=# CREATE USER testread with password 'test123';
                CREATE ROLE
                testdb=#

13.  дайте роль readonly пользователю testread

                testdb=# grant readonly TO testread;
                GRANT ROLE
                testdb=#

14.  зайдите под пользователем testread в базу данных testdb

                testdb=# \c testdb testread
                Password for user testread:
                You are now connected to database "testdb" as user "testread".
                testdb=>

15.  сделайте select * from t1;

                testdb=> SELECT * FROM t1;
                ERROR:  permission denied for table t1
                testdb=>

17.  получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже)
напишите что именно произошло в тексте домашнего задания
у вас есть идеи почему? ведь права то дали?
посмотрите на список таблиц
подсказка в шпаргалке под пунктом 20
а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально)

                testdb=> \dt
                        List of relations
                 Schema | Name | Type  |  Owner
                --------+------+-------+----------
                 public | t1   | table | postgres
                (1 row)
                
                testdb=>

Проблема с селектом, так как таблица создана в схеме public, а права мы дали для схемы testnm

22.  вернитесь в базу данных testdb под пользователем postgres

                testdb=> \c testdb postgres
                Password for user postgres:
                You are now connected to database "testdb" as user "postgres".
                testdb=#

23.  удалите таблицу t1

                testdb=# DROP TABLE t1;
                DROP TABLE
                testdb=#

24.  создайте ее заново но уже с явным указанием имени схемы testnm

                testdb=# CREATE TABLE testnm.t1(c1 integer);
                CREATE TABLE
                testdb=#

25.  вставьте строку со значением c1=1

                testdb=# INSERT INTO testnm.t1 values(1);
                INSERT 0 1
                testdb=# select * from testnm.t1;
                 c1
                ----
                  1
                (1 row)
                
                testdb=#


26.  зайдите под пользователем testread в базу данных testdb

                testdb=# ^C
                testdb=# \c testdb testread
                Password for user testread:
                You are now connected to database "testdb" as user "testread".
                testdb=>

27.  сделайте select * from testnm.t1;
получилось?
есть идеи почему?

                testdb=> select * from testnm.t1;
                ERROR:  permission denied for table t1
                testdb=>

та же самая ситуация, что была на вебинаре, мы сначала дали права, а потом создали таблицу

28. как сделать так чтобы такое больше не повторялось

надо опять зайти пользователем postgres и изменить права
grant usage on SCHEMA testnm to readonly;
grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
после чего переподключился testread

29. сделайте select * from testnm.t1;

получилось?
да, теперь получилось

                testdb=# \c testdb testread
                Password for user testread:
                You are now connected to database "testdb" as user "testread".
                testdb=>
                testdb=>
                testdb=> SELECT * FROM testnm.t1;
                 c1
                ----
                  1
                (1 row)
                
                testdb=>



30.  теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);

                testdb=> create table t2 (c1 integer);
                ERROR:  permission denied for schema public
                LINE 1: create table t2 (c1 integer);
                     ^
ситуация соответствует, прав на public не предоставлялась роли readonly



