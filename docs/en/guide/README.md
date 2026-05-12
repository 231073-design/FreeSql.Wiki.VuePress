CREATE TABLE Staff (
    sno        NUMBER(4) PRIMARY KEY,
    sname      VARCHAR2(30) NOT NULL,
    sal        NUMBER(7,2),
    DOB        DATE,
    gender     CHAR(1),
    ID         NUMBER(10),
    email      VARCHAR2(50),

    CONSTRAINT chk_salary
        CHECK (sal BETWEEN 0 AND 9999.99),

    CONSTRAINT chk_gender
        CHECK (gender IN ('M','F')),

    CONSTRAINT chk_age
        CHECK (DOB <= ADD_MONTHS(SYSDATE, -216))
);


/*====================================================
  CREATE TABLE: VACATION
====================================================*/
CREATE TABLE Vacation (
    sno        NUMBER(4),
    sdate      DATE,
    edate      DATE,
    notes      VARCHAR2(100),

    CONSTRAINT pk_vacation
        PRIMARY KEY (sno, sdate),

    CONSTRAINT fk_staff
        FOREIGN KEY (sno)
        REFERENCES Staff(sno),

    CONSTRAINT chk_dates
        CHECK (sdate < edate)
);


/*====================================================
  INSERT DATA INTO STAFF
====================================================*/
INSERT INTO Staff VALUES
(2442, 'Ruba', 5400.00, TO_DATE('1982-04-14','YYYY-MM-DD'), 'F', 4892785294, 'ruba@gmail.com');

INSERT INTO Staff VALUES
(4251, 'Ahmad', 3500.00, TO_DATE('1989-12-12','YYYY-MM-DD'), 'M', 1479358454, 'ahmad@gmail.com');

INSERT INTO Staff VALUES
(5638, 'Omar', 8700.50, TO_DATE('1984-03-23','YYYY-MM-DD'), 'M', 8904365783, 'omar@gmail.com');

INSERT INTO Staff VALUES
(9604, 'Jana', 2500.50, TO_DATE('2001-01-23','YYYY-MM-DD'), 'F', 5792957829, 'jana@gmail.com');

INSERT INTO Staff VALUES
(3424, 'Sara', 2750.00, TO_DATE('1999-05-19','YYYY-MM-DD'), 'F', 1782375892, 'sara@gmail.com');

INSERT INTO Staff VALUES
(2901, 'Ali', 7100.00, TO_DATE('1979-07-04','YYYY-MM-DD'), 'M', 9837583309, 'ali@gmail.com');


/*====================================================
  INSERT DATA INTO VACATION
====================================================*/
INSERT INTO Vacation VALUES
(2442, TO_DATE('2024-11-10','YYYY-MM-DD'),
       TO_DATE('2024-11-24','YYYY-MM-DD'),
       'Sickness');

INSERT INTO Vacation VALUES
(2442, TO_DATE('2025-09-12','YYYY-MM-DD'),
       TO_DATE('2025-09-19','YYYY-MM-DD'),
       'Trip');

INSERT INTO Vacation VALUES
(4251, TO_DATE('2026-03-25','YYYY-MM-DD'),
       TO_DATE('2026-03-28','YYYY-MM-DD'),
       'Eid Holiday');

INSERT INTO Vacation VALUES
(4251, TO_DATE('2025-07-10','YYYY-MM-DD'),
       TO_DATE('2025-07-20','YYYY-MM-DD'),
       'Family gather');

INSERT INTO Vacation VALUES
(4251, TO_DATE('2024-11-28','YYYY-MM-DD'),
       TO_DATE('2024-12-05','YYYY-MM-DD'),
       'Emergency');

INSERT INTO Vacation VALUES
(5638, TO_DATE('2024-03-23','YYYY-MM-DD'),
       TO_DATE('2024-05-17','YYYY-MM-DD'),
       'Medical treatment');

INSERT INTO Vacation VALUES
(5638, TO_DATE('2025-08-12','YYYY-MM-DD'),
       TO_DATE('2025-09-02','YYYY-MM-DD'),
       'Summer trip');

INSERT INTO Vacation VALUES
(9604, TO_DATE('2026-02-15','YYYY-MM-DD'),
       TO_DATE('2026-02-20','YYYY-MM-DD'),
       'Wedding');

INSERT INTO Vacation VALUES
(9604, TO_DATE('2026-04-03','YYYY-MM-DD'),
       TO_DATE('2026-05-03','YYYY-MM-DD'),
       'Honeymoon');

INSERT INTO Vacation VALUES
(3424, TO_DATE('2025-12-28','YYYY-MM-DD'),
       TO_DATE('2026-01-02','YYYY-MM-DD'),
       'Holiday');

INSERT INTO Vacation VALUES
(2901, TO_DATE('2024-10-15','YYYY-MM-DD'),
       TO_DATE('2024-10-25','YYYY-MM-DD'),
       'Emergency');

COMMIT;


/*====================================================
  QUERY 1
  List the names and salaries of all staff members
====================================================*/
SELECT sname, sal
FROM Staff;


/*====================================================
  QUERY 2
  Retrieve the staff number and name of all female employees
====================================================*/
SELECT sno, sname
FROM Staff
WHERE gender = 'F';


/*====================================================
  QUERY 3
  Show the staff number along with the start and end dates
  of all vacations
====================================================*/
SELECT sno, sdate, edate
FROM Vacation;


/*====================================================
  QUERY 4
  Display staff names, emails, and salaries sorted by salary
  in descending order
====================================================*/
SELECT sname, email, sal
FROM Staff
ORDER BY sal DESC;


/*====================================================
  QUERY 5
  List the names of staff members who have never taken
  a vacation
====================================================*/
SELECT sname
FROM Staff
WHERE sno NOT IN (
    SELECT sno
    FROM Vacation
);


/*====================================================
  QUERY 6
  Calculate the average salary for male and female staff
  separately
====================================================*/
SELECT gender, AVG(sal) AS avg_salary
FROM Staff
GROUP BY gender;


/*====================================================
  QUERY 7
  List staff names and the sum of their vacation days,
  put 0 for staff with no vacations
====================================================*/
SELECT s.sname,
       NVL(SUM(v.edate - v.sdate),0) AS total_days
FROM Staff s
LEFT JOIN Vacation v
ON s.sno = v.sno
GROUP BY s.sname;


/*====================================================
  QUERY 8
  List all the staff names with salary greater than
  the company's average salary
====================================================*/
SELECT sname
FROM Staff
WHERE sal > (
    SELECT AVG(sal)
    FROM Staff
);


/*====================================================
  QUERY 9
  Find vacations that lasted more than 10 days
====================================================*/
SELECT sno, sdate, edate
FROM Vacation
WHERE (edate - sdate) > 10;


/*====================================================
  QUERY 10
  For each staff member, show the staff member's name
  and the total number of vacation days taken
====================================================*/
SELECT s.sname,
       SUM(v.edate - v.sdate) AS total_vacation_days
FROM Staff s
JOIN Vacation v
ON s.sno = v.sno
GROUP BY s.sname;
