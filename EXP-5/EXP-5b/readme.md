# Experiment - 5b

## Create student data base
## it is already created in experiment-5a
## Do the Following
1. Truncate the table
2. Describe the student
3. Display the student table

```

TRUNCATE table student;
DESC student;
SELECT * FROM student;

```

![Output 1](5b1.png)
![output 2](5b2.png)

## PL/SQL Code that add Records in student Database

```
SET SERVEROUTPUT ON;

BEGIN
    -- Insert the first student record
    INSERT INTO student
    VALUES (116, 'Harish', 'CSE', 82);

    -- Insert the second student record
    INSERT INTO student
    VALUES (117, 'Lakshmi', 'ECE', 90);
    -- Create SAVEPOINT
    SAVEPOINT SP1;

    -- Insert the third student record
    INSERT INTO student
    VALUES (118, 'Naveen', 'IT', 75);

    -- Display message
    DBMS_OUTPUT.PUT_LINE('All three student records have been inserted.');

    -- Rollback the third record
    ROLLBACK TO SP1;

    -- Display rollback message
    DBMS_OUTPUT.PUT_LINE('Rollback to savepoint SP1 has been completed.');

    -- Permanently save the first two records
    COMMIT;

    -- Display commit message
    DBMS_OUTPUT.PUT_LINE('Transaction has been committed successfully.');

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM );
END;

```
## output of Pl/sql code
![pl/sql code output](5b3.png)

## Verify the results
![pl/sql results](5b4.png)

---

