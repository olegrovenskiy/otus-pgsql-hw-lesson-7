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

16.  сделайте select * from t1;

17.  получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже)
напишите что именно произошло в тексте домашнего задания
у вас есть идеи почему? ведь права то дали?
посмотрите на список таблиц
подсказка в шпаргалке под пунктом 20
а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально)


18.  вернитесь в базу данных testdb под пользователем postgres

19.  удалите таблицу t1

20.  создайте ее заново но уже с явным указанием имени схемы testnm

21.  вставьте строку со значением c1=1

22.  зайдите под пользователем testread в базу данных testdb

23.  сделайте select * from testnm.t1;
получилось?
есть идеи почему? если нет - смотрите шпаргалку
как сделать так чтобы такое больше не повторялось? если нет идей - смотрите шпаргалку
сделайте select * from testnm.t1;
получилось?
есть идеи почему? если нет - смотрите шпаргалку
сделайте select * from testnm.t1;
получилось?
ура!

24.  теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);
а как так? нам же никто прав на создание таблиц и insert в них под ролью readonly?
есть идеи как убрать эти права? если нет - смотрите шпаргалку
если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды
теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);
расскажите что получилось и почему
