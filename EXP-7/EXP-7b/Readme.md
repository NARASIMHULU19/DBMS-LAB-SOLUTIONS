# Experiment - 7b

## Program 1: Calculate Annual Salary Using a Stored Function 

## 1. Create the Employee table

```
CREATE TABLE employee (
    employee_id NUMBER(5) PRIMARY KEY,
    employee_name VARCHAR2(50),
    monthly_salary NUMBER(10,2)
);

```
## Output of Employee table

![output](7b1.png)

## 2. Insert Sample Employee Records

```
INSERT INTO employee VALUES (101, 'Ravi', 25000);
INSERT INTO employee VALUES (102, 'Sita', 30000);
INSERT INTO employee VALUES (103, 'Kiran', 35000);
INSERT INTO employee VALUES (104, 'Anjali', 40000);
INSERT INTO employee VALUES (105, 'Rahul', 45000);

COMMIT;

```
## Output of Insertion Employee table

![output](7b2.png)

## 3. Create the Stored Function

```
CREATE OR REPLACE FUNCTION CALCULATE_ANNUAL_SALARY (
    p_monthly_salary IN NUMBER
)
RETURN NUMBER
IS
    v_annual_salary NUMBER;
BEGIN
    -- Calculate annual salary
    v_annual_salary := p_monthly_salary * 12;

    -- Return annual salary
    RETURN v_annual_salary;
END;

```
## Output of Compiled Function

![output](7b3.png)

## Execute the Function Using SELECT

```

SELECT
    employee_id,
    employee_name,
    monthly_salary,
    CALCULATE_ANNUAL_SALARY(monthly_salary) AS annual_salary
FROM employee;

```
## Output of Function using Select Statement

![output](7b1.png)

---

