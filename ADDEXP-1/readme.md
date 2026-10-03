# Additional Experiment -1
# Student Information Lookup Using Student ID 
## Create the STUDENT Table

```
CREATE TABLE student (
    student_id   NUMBER(5) PRIMARY KEY,
    student_name VARCHAR2(50),
    department   VARCHAR2(30),
    marks        NUMBER(5,2)
);

```
### Output
![output](a1a.png)
## Insert Sample Records

```
INSERT INTO student VALUES (101, 'Ravi',   'CSE', 85);
INSERT INTO student VALUES (102, 'Sita',   'ECE', 92);
INSERT INTO student VALUES (103, 'Kiran',  'EEE', 78);
INSERT INTO student VALUES (104, 'Anjali', 'CSE', 88);
INSERT INTO student VALUES (105, 'Rahul',  'IT',  74);

COMMIT;

```
### Output
![output](a1b.png)


## Verify the records

```
SELECT * FROM student;

```
### Output
![output](a1c.png)


## Write the PL/SQL Block

```
SET SERVEROUTPUT ON;

DECLARE
    v_student_id   student.student_id%TYPE;
    v_student_name student.student_name%TYPE;
    v_department   student.department%TYPE;
    v_marks        student.marks%TYPE;

BEGIN
    -- Accept Student ID from the user
    v_student_id := &student_id;

    -- Retrieve student details
    SELECT student_name, department, marks
    INTO v_student_name, v_department, v_marks
    FROM student
    WHERE student_id = v_student_id;

    -- Display student details
    DBMS_OUTPUT.PUT_LINE('Student ID   : ' || v_student_id);
    DBMS_OUTPUT.PUT_LINE('Student Name : ' || v_student_name);
    DBMS_OUTPUT.PUT_LINE('Department   : ' || v_department);
    DBMS_OUTPUT.PUT_LINE('Marks        : ' || v_marks);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE(
            'No student found with Student ID: ' || v_student_id
        );

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(
            'Error: ' || SQLERRM
        );
END;
/

```

### Output
![output](a1d.png)



## Execute for an Invalid Student ID

### Output
![output](a1e.png)


### Output
![output](a1f.png)



