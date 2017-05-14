### CREATE 'MYDB' databse
    createdb mydb
### DESTROY database
    dropdb mydb
### ACTIVATE mydb
    psql mydb
    output : psql (9.6.3)
             Type "help" for help.

             mydb=>
### HELP page
    psql --help
### EXIT 
    \q
### LOGGING into database
    psql -U postgres mydb
### CREATING realation
    CREATE TABLE weather (
    city            varchar(80),
    temp_lo         int,           -- low temperature
    temp_hi         int,           -- high temperature
    prcp            real,          -- precipitation
    date            date
    );
### TO DELETE table
    drop table tablename
### POPULATING NEW TABLE
    INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');

### Use COPY to load large amounts of data from flat-text files
    COPY weather FROM '/home/user/weather.txt';
    
### Querying a Table -To retrieve data from a table, the table is queried. type:
    SELECT * FROM weather;

### JOINS BETWEEN TABLES
    
    1. SELECT * FROM weather, cities WHERE city = name;
    
    2. SELECT weather.city, weather.temp_lo, weather.temp_hi,
       weather.prcp, weather.date, cities.location
    FROM weather, cities
    WHERE cities.name = weather.city;

    3. SELECT *
    FROM weather INNER JOIN cities ON (weather.city = cities.name);
    
    
### AGGREGATE functions
    SELECT max(temp_lo) FROM weather;

    max
    -----
    46
    (1 row)
### UPADTE 
    UPDATE weather
    SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
    WHERE date > '1994-11-28';
### DELETIONS
    DELETE FROM weather WHERE city = 'Hayward';
    
### VIEWS
    CREATE VIEW myview AS
    SELECT city, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

    SELECT * FROM myview;
### FOREIGN KEYS
    CREATE TABLE cities (
        city     varchar(80) primary key,
        location point
    );

    CREATE TABLE weather (
        city      varchar(80) references cities(city),
        temp_lo   int,
        temp_hi   int,
        prcp      real,
        date      date
    );
    
### TRANSACTIONS
    BEGIN;
    UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
    -- etc etc
    COMMIT;
    
    
    BEGIN;
    UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
    SAVEPOINT my_savepoint;
    UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Bob';
    -- oops ... forget that and use Wally's account
    ROLLBACK TO my_savepoint;
    UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Wally';
    COMMIT;
    
### WINDOWS FUNCTIONS
    SELECT depname, empno, salary, avg(salary) OVER (PARTITION BY depname) FROM empsalary;
### INHERITANCE
    CREATE TABLE cities (
    name       text,
    population real,
    altitude   int     -- (in ft)
    );

    CREATE TABLE capitals (
    state      char(2)
    ) INHERITS (cities);
