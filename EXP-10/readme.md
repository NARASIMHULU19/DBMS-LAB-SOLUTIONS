# EXPERIMENT - 10

# Title: Indexing Techniques for Database Performance 

## Create the EMPLOYEE table

```
CREATE TABLE employee (
    employee_id   NUMBER(6) PRIMARY KEY,
    employee_name VARCHAR2(50),
    department    VARCHAR2(30),
    salary        NUMBER(10,2)
);

```
### Output
![output](10a.png)

## Insert sample employee records

```
INSERT INTO employee VALUES (1001, 'Ravi',   'CSE', 30000);
INSERT INTO employee VALUES (1002, 'Sita',   'ECE', 35000);
INSERT INTO employee VALUES (1003, 'Kiran',  'EEE', 40000);
INSERT INTO employee VALUES (1004, 'Anjali', 'CSE', 45000);
INSERT INTO employee VALUES (1005, 'Rahul',  'ECE', 38000);
INSERT INTO employee VALUES (1006, 'Priya',  'CSE', 50000);
INSERT INTO employee VALUES (1007, 'Arun',   'EEE', 42000);
INSERT INTO employee VALUES (1008, 'Sneha',  'CSE', 48000);
INSERT INTO employee VALUES (1009, 'Vijay',  'ECE', 36000);
INSERT INTO employee VALUES (1010, 'Divya',  'CSE', 52000);

COMMIT;

```
### Output
![output](10b.png)


## Verify the employee 

```
SELECT * FROM employee;

```
### Output
![output](10c.png)


## Execute search query without an index

```
SELECT *
FROM employee
WHERE employee_name = 'Ravi';

```

### Output
![output](10d.png)


## Display the execution plan

```
EXPLAIN PLAN FOR
SELECT *
FROM employee
WHERE employee_name = 'Ravi';

SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);

```
### Output
![output](10e.png)


## Create an index on the search column

```
CREATE INDEX idx_employee_name
ON employee(employee_name);

```
### Output
![output](10f.png)


## Execute the same search query again

```
SELECT *
FROM employee
WHERE employee_name = 'Ravi';

```
### Output
![output](10g.png)


## Display the execution plan
### First, gather table statistics:
```
BEGIN
    DBMS_STATS.GATHER_TABLE_STATS(
        USER,
        'EMPLOYEE'
    );
END;
/
```
### Output
![output](10h.png)

### Now generate the execution plan again

```
EXPLAIN PLAN FOR
SELECT *
FROM employee
WHERE employee_name = 'Ravi';

SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);

```
### Output
![output](10i.png)

## Drop the created index

```
DROP INDEX idx_employee_name;

```

### Output
![output](10j.png)

### Verify that the index has been removed

```
SELECT index_name
FROM user_indexes
WHERE index_name = 'IDX_EMPLOYEE_NAME';

```

### Output
![output](10k.png)








