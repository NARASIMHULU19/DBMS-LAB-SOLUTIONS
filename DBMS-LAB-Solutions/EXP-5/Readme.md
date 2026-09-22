# Q5 1) Write a PL/SQL program to retrieve and display the details of students who have secured First  Class (Marks ≥ 60) from the STUDENT table using an explicit cursor. The program should declare a user-defined exception that is raised when no student satisfies the condition. Display the Student ID, Student Name, and Marks for all match ing students using DBMS\_OUTPUT.PUT\_LINE. Handle the user-defined exception by dis playing the message "No First  Class Students Found." Also, handle any unexpected runtime errors using the WHEN OTHERS  exception handler.

# Plsql Code

'''
SET SERVEROUTPUT ON;

DECLARE
-- Boolean variable to check whether any student is found
v\_found BOOLEAN := FALSE;

&#x20;   -- User-defined exception
    e\_no\_first\_class EXCEPTION;

    -- Cursor to retrieve First Class students
    CURSOR c\_first\_class IS
        SELECT student\_id, student\_name, marks
        FROM student
        WHERE marks >= 60;


BEGIN
-- Open cursor and process each student
FOR student\_rec IN c\_first\_class
LOOP
-- A matching record is found
v\_found := TRUE;

&#x20;       -- Display student details
        DBMS\_OUTPUT.PUT\_LINE(
            'Student ID   : ' || student\_rec.student\_id
        );

        DBMS\_OUTPUT.PUT\_LINE(
            'Student Name : ' || student\_rec.student\_name
        );

        DBMS\_OUTPUT.PUT\_LINE(
            'Marks        : ' || student\_rec.marks
        );

        DBMS\_OUTPUT.PUT\_LINE('---------------------------');
    END LOOP;

    -- Check whether any record was found
    IF v\_found = FALSE THEN
        RAISE e\_no\_first\_class;
    END IF;


EXCEPTION
-- Handle user-defined exception
WHEN e\_no\_first\_class THEN
DBMS\_OUTPUT.PUT\_LINE('No First Class Students Found.');

&#x20;   -- Handle other unexpected exceptions
    WHEN OTHERS THEN
        DBMS\_OUTPUT.PUT\_LINE(
            'Error: ' || SQLERRM
        );

END;
/
'''

