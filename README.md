# DBMS-LAB-PRGMCREATE
TABLE BRANCH (

    Branchid INT PRIMARY KEY,

    Branchname VARCHAR(100),

    HOD VARCHAR(100)

);
CREATE TABLE STUDENT (

    USN VARCHAR(10) PRIMARY KEY,

    Name VARCHAR(100),

    Address VARCHAR(200),

    Branchid INT,

    sem INT,

    FOREIGN KEY (Branchid) REFERENCES BRANCH(Branchid)

);

CREATE TABLE AUTHOR (

    Authorid INT PRIMARY KEY,

    Authorname VARCHAR(100),

    Country VARCHAR(50),

    age INT

);

CREATE TABLE BOOK (

    Bookid INT PRIMARY KEY,

    Bookname VARCHAR(200),

    Authorid INT,

    Publisher VARCHAR(100),

    Branchid INT,

    FOREIGN KEY (Authorid) REFERENCES AUTHOR(Authorid),

    FOREIGN KEY (Branchid) REFERENCES BRANCH(Branchid)

);
CREATE TABLE BORROW (

    USN VARCHAR(10),

    Bookid INT,

    Borrowed_Date DATE,

    PRIMARY KEY (USN, Bookid),

    FOREIGN KEY (USN) REFERENCES STUDENT(USN),

    FOREIGN KEY (Bookid) REFERENCES BOOK(Bookid)

);

BRANCH TABLE:
INSERT INTO BRANCH (Branchid, Branchname, HOD) VALUES (1, 'MCA', 'NPK');

INSERT INTO BRANCH (Branchid, Branchname, HOD) VALUES (2, 'MBA', 'Bojanna');

INSERT INTO BRANCH (Branchid, Branchname, HOD) VALUES (3, 'CSE', 'GTR');

INSERT INTO BRANCH (Branchid, Branchname, HOD) VALUES (4, 'ISE', 'Sudhamani');

INSERT INTO BRANCH (Branchid, Branchname, HOD) VALUES (5, 'Electrical', 'Sumathi');

STUDENT TABLE:

INSERT INTO STUDENT (USN, Name, Address, Branchid, sem) VALUES ('1rn1', 'Harish', 'Bangalore', 1, 2);

INSERT INTO STUDENT (USN, Name, Address, Branchid, sem) VALUES ('1rn2', 'Bharath', 'Mysore', 2, 3);

INSERT INTO STUDENT (USN, Name, Address, Branchid, sem) VALUES ('1rn3', 'Kiran', 'Delhi', 3, 6);

INSERT INTO STUDENT (USN, Name, Address, Branchid, sem) VALUES ('1rn4', 'Mahi', 'Chennai', 4, 7);

INSERT INTO STUDENT (USN, Name, Address, Branchid, sem) VALUES ('1rn5', 'Krishna', 'Hubli', 5, 4);

AUTHOR TABLE

INSERT INTO AUTHOR (Authorid, Authorname, Country, age) VALUES (123, 'Navathe', 'India', 55);

INSERT INTO AUTHOR (Authorid, Authorname, Country, age) VALUES (124, 'Ritche', 'UK', 44);

INSERT INTO AUTHOR (Authorid, Authorname, Country, age) VALUES (125, 'Ramkrishna', 'India', 55);

INSERT INTO AUTHOR (Authorid, Authorname, Country, age) VALUES (126, 'Sumitabha', 'India', 38);

INSERT INTO AUTHOR (Authorid, Authorname, Country, age) VALUES (127, 'Dennis', 'USA', 66);

BOOK TABLE

INSERT INTO BOOK (Bookid, Bookname, Authorid, Publisher, Branchid) VALUES (1111, 'C Prog', 123, 'Pearson', 1);

INSERT INTO BOOK (Bookid, Bookname, Authorid, Publisher, Branchid) VALUES (2222, 'DBMS', 124, 'McGrawHill', 2);

INSERT INTO BOOK (Bookid, Bookname, Authorid, Publisher, Branchid) VALUES (3333, 'OOPS', 125, 'Sapna', 3);

INSERT INTO BOOK (Bookid, Bookname, Authorid, Publisher, Branchid) VALUES (4444, 'Unix', 126, 'Subhash', 4);

INSERT INTO BOOK (Bookid, Bookname, Authorid, Publisher, Branchid) VALUES (5555, 'C Prog', 127, 'Pearson', 5);

BORROW TABLE:

INSERT INTO BORROW (USN, Bookid, Borrowed_Date) VALUES ('1rn1', 2222, '2000-01-10');

INSERT INTO BORROW (USN, Bookid, Borrowed_Date) VALUES ('1rn1', 3333, '2016-03-05');

INSERT INTO BORROW (USN, Bookid, Borrowed_Date) VALUES ('1rn3', 5555, '2010-06-01');

INSERT INTO BORROW (USN, Bookid, Borrowed_Date) VALUES ('1rn5', 2222, '2000-05-19');

INSERT INTO BORROW (USN, Bookid, Borrowed_Date) VALUES ('1rn2', 1111, '2015-02-22');

Queries:

1.	List the details of Students who are all studying in 2nd sem MCA.

SELECT * 
FROM STUDENT 
WHERE sem = 2 AND Branchid = (SELECT Branchid FROM BRANCH WHERE Branchname = 'MCA');

2.	List the students who are not borrowed any books.

SELECT * 
FROM STUDENT 
WHERE USN NOT IN (SELECT USN FROM BORROW);

3.	 Display the USN, Student name, Branch_name, Book_name, Author_name, Books_Borrowed_Date of 2nd sem MCA Students who borrowed books.

SELECT S.USN, S.Name, B.Branchname, BK.Bookname, A.Authorname, BR.Borrowed_Date

FROM STUDENT S

JOIN BRANCH B ON S.Branchid = B.Branchid

JOIN BORROW BR ON S.USN = BR.USN

JOIN BOOK BK ON BR.Bookid = BK.Bookid

JOIN AUTHOR A ON BK.Authorid = A.Authorid

WHERE S.sem = 2 AND B.Branchname = 'MCA';

4.	Display the number of books written by each Author.

select count(*) , authorid from book group by authorid;

5.	Display the student details who borrowed more than two books.

select * from student where usn in ( select usn from borrow group by usn having count(usn) >=2);

6.	Display the student details who borrowed books of more than one Author.

SELECT S.*

FROM STUDENT S

WHERE S.USN IN (

    SELECT BR.USN

    FROM BORROW BR

    JOIN BOOK BK ON BR.Bookid = BK.Bookid

    GROUP BY BR.USN

    HAVING COUNT(DISTINCT BK.Authorid) > 1

);	

7.	Display the Book names in descending order of their names.

SELECT Bookname

FROM BOOK

ORDER BY Bookname DESC;

8.	List the details of students who borrowed the books which are all published by the same publisher.

select * from student s where exists (select usn , publisher from borrow

 join book on borrow.bookid=book.bookid

 where s.usn=borrow.usn

 group by usn 

having count(distinct publisher)=1);

