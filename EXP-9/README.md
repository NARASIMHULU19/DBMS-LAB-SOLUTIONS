# EXPERIMENT - 9
# Trigger Programming in PL/SQL
## Program 1: BEFORE Trigger
### 1. Create the STUDENT Table

```

CREATE TABLE student (
    student_id   NUMBER(5) PRIMARY KEY,
    student_name VARCHAR2(50),
    course       VARCHAR2(30),
    marks        NUMBER(5,2)
);

```
### Output
![output](9.1a.png)

### 2. Create the BEFORE INSERT Trigger

```
CREATE OR REPLACE TRIGGER trg_student_before_insert
BEFORE INSERT ON student
FOR EACH ROW
BEGIN

    -- Validate Student ID
    IF :NEW.student_id <= 0 THEN
        RAISE_APPLICATION_ERROR(
            -20001,
            'Student ID must be greater than 0.'
        );
    END IF;

    -- Validate Student Name
    IF :NEW.student_name IS NULL THEN
        RAISE_APPLICATION_ERROR(
            -20002,
            'Student Name cannot be NULL.'
        );
    END IF;

    -- Validate Marks
    IF :NEW.marks < 0 OR :NEW.marks > 100 THEN
        RAISE_APPLICATION_ERROR(
            -20003,
            'Marks must be between 0 and 100.'
        );
    END IF;

END;
/

``` 
### Output
![output](9.1b.png)



### 3. Insert a Valid Record

```

INSERT INTO student
VALUES (101, 'Ravi', 'CSE', 85);

COMMIT;

```

### Output
![output](9.1c.png)



### 4. Insert an Invalid Record

```
INSERT INTO student
VALUES (102, 'Sita', 'ECE', 120);

```
### Output
![output](9.1d.png)



### 5. Test Another Invalid Record

```
INSERT INTO student
VALUES (-103, 'Kiran', 'EEE', 75);

```
### Output
![output](9.1e.png)

### 6. Display the Final Table

```
SELECT * FROM student;

```
### Output
![output](9.1f.png)

---

## Program 2: AFTER Trigger

### 1. Create the Main Table

```

CREATE TABLE student (
    student_id   NUMBER(5) PRIMARY KEY,
    student_name VARCHAR2(50),
    course       VARCHAR2(30),
    marks        NUMBER(5,2)
);

```

### Output
![output](9.2a.png)


### 2. Create the Audit Table

```
CREATE TABLE student_audit (
    audit_id      NUMBER(5),
    student_id    NUMBER(5),
    student_name  VARCHAR2(50),
    course        VARCHAR2(30),
    marks         NUMBER(5,2),
    action        VARCHAR2(20),
    action_date   DATE
);

```
### Output
![output](9.2b.png)


### 3. Create a Sequence for the Audit ID

```
CREATE SEQUENCE student_audit_seq
START WITH 1
INCREMENT BY 1;

```
### Output
![output](9.2c.png)


### 4. Create the AFTER INSERT Trigger

```

CREATE OR REPLACE TRIGGER trg_student_after_insert
AFTER INSERT ON student
FOR EACH ROW
BEGIN

    INSERT INTO student_audit (
        audit_id,
        student_id,
        student_name,
        course,
        marks,
        action,
        action_date
    )
    VALUES (
        student_audit_seq.NEXTVAL,
        :NEW.student_id,
        :NEW.student_name,
        :NEW.course,
        :NEW.marks,
        'INSERT',
        SYSDATE
    );

END;
/

```
### Output
![output](9.2d.png)



### 5. Insert a New Record into the Main Table

```
INSERT INTO student
VALUES (101, 'Ravi', 'CSE', 85);

COMMIT;

```
### Output
![output](9.2e.png)

### 6. Verify the Main Table

```
SELECT * FROM student;

```
### Output
![output](9.2f.png)


### 7. Display the Audit Table

```
SELECT * FROM student_audit;

```
### Output
![output](9.2g.png)

---

## Program 3: Row-Level Trigger 

### 1. Create the EMPLOYEE Table

```
CREATE TABLE employee (
    employee_id   NUMBER(5) PRIMARY KEY,
    employee_name VARCHAR2(50),
    department    VARCHAR2(30),
    salary        NUMBER(10,2)
);

```

### Output
![output](9.3a.png)


### 2. Insert Sample Employee Records

```
INSERT INTO employee VALUES (101, 'Ravi', 'CSE', 30000);
INSERT INTO employee VALUES (102, 'Sita', 'ECE', 35000);
INSERT INTO employee VALUES (103, 'Kiran', 'EEE', 40000);
INSERT INTO employee VALUES (104, 'Anjali', 'CSE', 45000);

COMMIT;
```

### Output
![output](9.3b.png)


### 3. Create the BEFORE UPDATE Trigger

```

CREATE OR REPLACE TRIGGER trg_employee_before_update
BEFORE UPDATE ON employee
FOR EACH ROW
BEGIN

    -- Compare old and new salary
    IF :NEW.salary < :OLD.salary THEN

        RAISE_APPLICATION_ERROR(
            -20001,
            'Salary cannot be decreased.'
        );

    END IF;

END;
/

```

### Output
![output](9.3c.png)



### 4. Update a Valid Record

```
UPDATE employee
SET salary = 33000

WHERE employee_id = 101;

COMMIT;

```

### Output
![output](9.3d.png)


### Verify:

```
SELECT * FROM employee
WHERE employee_id = 101;

```

### Output
![output](9.3e.png)


### 5. Update an Invalid Record

```

UPDATE employee
SET salary = 28000
WHERE employee_id = 101;

```


### Output
![output](9.3f.png)


### 6. Display the Table Contents

```
SELECT * FROM employee;

```
### Output
![output](9.3g.png)

---


## Program 4: Statement-Level Trigger 

### 1. Create the Main Table

```

CREATE TABLE employee (
    employee_id   NUMBER(5) PRIMARY KEY,
    employee_name VARCHAR2(50),
    department    VARCHAR2(30),
    salary        NUMBER(10,2)
);

```
### Output
![output](9.4a.png)


### 2. Insert Sample Records

```

INSERT INTO employee VALUES (101, 'Ravi', 'CSE', 30000);
INSERT INTO employee VALUES (102, 'Sita', 'ECE', 35000);
INSERT INTO employee VALUES (103, 'Kiran', 'EEE', 40000);
INSERT INTO employee VALUES (104, 'Anjali', 'CSE', 45000);
INSERT INTO employee VALUES (105, 'Rahul', 'ECE', 38000);

COMMIT;

```

### Output
![output](9.4b.png)

### 3. Create the Log Table

```
CREATE TABLE employee_delete_log (
    log_id        NUMBER(5),
    message       VARCHAR2(200),
    delete_date   DATE
);

```

### Output
![output](9.4c.png)


### 4. Create a Sequence for the Log ID

```
CREATE SEQUENCE employee_delete_log_seq
START WITH 1
INCREMENT BY 1;

```
### Output
![output](9.4d.png)


### 5. Create the AFTER DELETE Statement-Level Trigger

```
CREATE OR REPLACE TRIGGER trg_employee_after_delete
AFTER DELETE ON employee
BEGIN

    INSERT INTO employee_delete_log (
        log_id,
        message,
        delete_date
    )
    VALUES (
        employee_delete_log_seq.NEXTVAL,
        'DELETE statement executed on EMPLOYEE table.',
        SYSDATE
    );

    DBMS_OUTPUT.PUT_LINE(
        'DELETE statement executed successfully.'
    );

END;

```

### Output
![output](9.4e.png)


### 6. Verify the Trigger

```

SELECT trigger_name, status
FROM user_triggers
WHERE trigger_name = 'TRG_EMPLOYEE_AFTER_DELETE';

```

### Output
![output](9.4f.png)


###  7. Delete One Record


```
SET SERVEROUTPUT ON;

DELETE FROM employee
WHERE employee_id = 101;

COMMIT;

```

### Output
![output](9.4g.png)


### 8. Verify the DELETE Operation

```
SELECT * FROM employee;

```

### Output
![output](9.4h.png)


### 9. Test Statement-Level Behavior

```
DELETE FROM employee
WHERE department = 'ECE';

COMMIT;

```
### Output
![output](9.4i.png)


### 10. Display the Log Table

```
SELECT * FROM employee_delete_log;

```

### Output
![output](9.4j.png)


## Program 5: INSTEAD OF Trigger 

### 1. Create the Base Tables

#### Create COURSE Table

```

CREATE TABLE course (
    course_id   NUMBER(5) PRIMARY KEY,
    course_name VARCHAR2(50)
);

```

### Output
![output](9.5a.png)


#### Create STUDENT Table

```

CREATE TABLE student (
    student_id   NUMBER(5) PRIMARY KEY,
    student_name VARCHAR2(50),
    course_id    NUMBER(5),
    marks        NUMBER(5,2),
    CONSTRAINT fk_student_course
        FOREIGN KEY (course_id)
        REFERENCES course(course_id)
);

```

### Output
![output](9.5b.png)


### 2. Insert Sample Records

#### Insert Course Records

```
INSERT INTO course VALUES (1, 'Computer Science');
INSERT INTO course VALUES (2, 'Electronics');
INSERT INTO course VALUES (3, 'Electrical');

COMMIT;

```

### Output
![output](9.5c.png)


#### Insert Student Records

```
INSERT INTO student VALUES (101, 'Ravi', 1, 85);
INSERT INTO student VALUES (102, 'Sita', 2, 90);
INSERT INTO student VALUES (103, 'Kiran', 3, 78);
INSERT INTO student VALUES (104, 'Anjali', 1, 88);

COMMIT;

```
### Output
![output](9.5d.png)


### 3. Create a View

```
CREATE OR REPLACE VIEW student_course_view AS
SELECT
    s.student_id,
    s.student_name,
    s.course_id,
    c.course_name,
    s.marks
FROM student s
JOIN course c
    ON s.course_id = c.course_id;

```

### Output
![output](9.5e.png)


#### Display the View

```
SELECT * FROM student_course_view;

```

### Output
![output](9.5f.png)


### 4. Create the INSTEAD OF UPDATE Trigger

```

CREATE OR REPLACE TRIGGER trg_student_view_update
INSTEAD OF UPDATE ON student_course_view
FOR EACH ROW
BEGIN

    UPDATE student
    SET
        student_name = :NEW.student_name,
        marks = :NEW.marks
    WHERE student_id = :OLD.student_id;

END;

```

### Output
![output](9.5g.png)


### 5. Update the View

```
UPDATE student_course_view
SET marks = 95
WHERE student_id = 101;

COMMIT;

```

### Output
![output](9.5h.png)


### 6. Verify the Base Table

```
SELECT * FROM student;

```

### Output
![output](9.5i.png)


### 7. Verify the View

```
SELECT * FROM student_course_view;

```

### Output
![output](9.5j.png)






















