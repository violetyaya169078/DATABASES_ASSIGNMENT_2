# DATABASES_ASSIGNMENT_2
Databases Assignment 2 Group Task(QUT, Semester A, 2022)
--*SQL*--

## Table of Contents
 <ul>
<li><a href="#Task1">Task 1</a></li>
<li><a href="#Task2">Task 2</a></li>
<li><a href="#Task3">Task 3</a></li>
<li><a href="#Task4">Task 4</a></li>
<li><a href="#Task5">Task 5</a></li>
 </ul>

<a id='Task1'></a>
### Task 1
Write an SQL script that builds a database to match the relational model below. These SQL
statements in the script must be provided in the correct order. You are required to create a database
for the fictitious bookstore Oktomook for Task 1. The database is based on the following model.

**THE OKTOMOOK RELATIONAL MODEL:**

* __Branch__ (<ins>branchNo</ins>, bName, bStreetNo, bStreetName, bPostCode, bState, numberEmployees)
* __Publisher__ (<ins>publisherNo</ins>, pName, pStreetNo, pStreetName, pPostCode, pState)
* __Author__ (<ins>authorID</ins>, aFirstName, aLastName)
* __Book__ (<ins>ISBN</ins>, title, publisherNo, genre, retailPrice, paperback)
* __Wrote__ (<ins>ISBN, authorID</ins>)
* __Inventory__ (<ins>ISBN, branchNo</ins>, quantityInStock) 

**PRIMARY KEYS** are denoted by <ins>bold and underline</ins>

**FOREIGN KEYS**
* Book(publisherNo) is dependent on Publisher (publisherNo)
* Wrote (ISBN) is dependent on Book (ISBN)
* Wrote (authorID) is dependent on Author (authorID)
* Inventory (ISBN) is dependent on Book (ISBN)
* Inventory (branchNo) is dependent on Branch (branchNo) 

**Other Constraints and Remarks**
* branchNo is a string comprising three digits.
* ISBN is a string comprising 10 digits.
* publisherNo is a string comprising one letter and two digits.
* authorID is a string comprising four digits.
* PostCode is a string comprising four digits.
* State is a string with three letter or less letters
* INTEGER type must be used for retailPrice, numberEmployees and quantityInStock.
* TEXT type must be used for other attributes (except retailPrice, numberEmployees and quantityInStock).
* Book title must contain a value. 

```
CREATE TABLE Branch (
branchNo		TEXT CHECK(LENGTH(branchNo)=3) NOT NULL,
bName			TEXT,
bStreetNo		TEXT,
bStreetName		TEXT,
bPostCode		TEXT CHECK(LENGTH(bPostCode)=4),
bState	 		TEXT CHECK(LENGTH(bState)<=3),
numberEmployees INTEGER,
PRIMARY KEY(branchNo))
```

```
CREATE TABLE Publisher (
publisherNo		TEXT CHECK (publisherNo glob '[a-zA-Z][0-9][0-9]') NOT NULL, 
pName			TEXT,
pStreetNo		TEXT,
pStreetName		TEXT,
pPostCode		TEXT CHECK(LENGTH(pPostCode)=4),
pState	 		TEXT CHECK(LENGTH(pState)<=3),
PRIMARY KEY(publisherNo))
```

```
CREATE TABLE Author (
authorID	TEXT CHECK(LENGTH(authorID)=4),
aFirstName	TEXT,
aLastName	TEXT,
PRIMARY KEY(authorID))
```

```
CREATE TABLE Book (
ISBN		TEXT CHECK(LENGTH(ISBN)=10),
title		TEXT NOT NULL,
publisherNo	TEXT CHECK (publisherNo glob '[a-zA-Z][0-9][0-9]'),
genre		TEXT,
retailPrice	INTEGER,
paperback	TEXT,
PRIMARY KEY(ISBN),
FOREIGN KEY(publisherNo) references Publisher (publisherNo))
```

```
CREATE TABLE Wrote (
ISBN		TEXT CHECK(LENGTH(ISBN)=10),
authorID	TEXT CHECK(LENGTH(authorID)=4),
PRIMARY KEY(ISBN, authorID),
FOREIGN KEY(ISBN) references Book (ISBN),
FOREIGN KEY(authorID) references Author (authorID))
```

```
CREATE TABLE Inventory (
ISBN			TEXT CHECK(LENGTH(ISBN)=10),
branchNo		TEXT CHECK(LENGTH(branchNo)=3),
quantityInStock	INTEGER,
PRIMARY KEY(ISBN, branchNo),
FOREIGN KEY(ISBN) references Book (ISBN),
FOREIGN KEY(branchNo) references Branch (branchNo))
```

<a id='Task2'></a>
### Task 2
We have provided you the Hotel database (Filename: Hotel IFN554 22s1.db) to be used with SQLite DBMS. You should use this database in SQLite to extract the
necessary information as per the following query requirements. 

The script is based on the following relational schema:
* Hotel (hotelNo, hotelName, city)
* Room (roomNo, hotelNo, type, price)
* Booking (hotelNo, guestNo, dateFrom, dateTo, roomNo)
* Guest (guestNo, guestName, guestAddress) 

**PRIMARY KEYS** are denoted by <ins>bold and underline</ins>

**FOREIGN KEYS** are denoted by *italics* and maybe part of a primary key

Write an SQL script for querying data for the following information.
1. List the hotelNo, type and price of each room that is a double, self or deluxe with a price
more than $110.
2. List the hotelNo which have 2 or more double rooms.
3. How many different guests visited the Grosvener Hotel?
4. What is the total income from bookings for the Grosvenor Hotel?
5. List all the guests’ names who have stayed in a hotel.

```
SELECT hotelNo, type, price FROM Room
WHERE type IN ('Double', 'Self', 'Deluxe')
AND price >110
```

```
SELECT hotelNo FROM Room
WHERE type = 'Double'
GROUP BY hotelNo HAVING COUNT (hotelNo)>1
```

```
SELECT COUNT(DISTINCT Booking.guestNo) AS guestVisit FROM Booking
Where hotelNo = 'H1'
```

```
SELECT SUM(price) AS totalIncome FROM Room
WHERE hotelNo = 'H1'
```

```
SELECT DISTINCT Guest.guestName
FROM Guest
JOIN Booking
ON Guest.guestNo = Booking.guestNo
```

<a id='Task3'></a>
### Task 3
Perform the following tasks.
* Write commands to insert one row of data in each of the Hotel database tables.
* Write a command to delete the row you inserted in the table Guest.
* Write a command to update the price of all rooms by 15%.

```
INSERT INTO Hotel VALUES('H8', 'Hilton Surfers', 'Surfers Paradise');
INSERT INTO Room VALUES('R100', 'H8', 'Single', 600);
INSERT INTO Guest VALUES('G300', 'Patrick Star', 'Wallaby Way');
INSERT INTO Booking VALUES('H8', 'G300', '2020-03-01', '2020-03-20', 'R100')
```

```
pragma foreign_keys = 0
DELETE FROM Guest WHERE guestNo = 'G300'
--pragma foreign_keys = 1
```

```
UPDATE Room 
SET price = price*1.15
```

<a id='Task4'></a>
### Task 4
Perform the following tasks.
* Currently the database only contains a small number of records, however the data contained
within it is expected to grow significantly in the future. Creating indexes on commonly searched
columns is a way performance issues can be minimised. Write a command to create an index on
guestName of the Guest table.
* Write a command to create a view to list the information (hotelName, roomType and the total
number of rooms booked) of the hotels which are in Cairns.

```
CREATE INDEX GuestIndex ON Guest (guestName)
```

```
CREATE VIEW CairnsHotel AS
SELECT  b.hotelName, c.type, COUNT(*) AS  roomBooked
FROM Booking a
INNER JOIN Hotel b
ON a.hotelNo = b.hotelNo
INNER JOIN Room c
ON a.hotelNo = c.hotelNo
AND a.roomNo = c.roomNo
WHERE b.city = 'Cairns'
GROUP BY b.hotelName, c.type
ORDER BY b.hotelName
```

<a id='Task5'></a>
### Task 5
Nikki and Phil work with the Hotel database as database administrator. Provide the commands
required to grant or revoke access so the following security requirements are met:

Perform the following tasks.
(*Note: You will not be able to try these commands in SQLite.*)
* User Nikki must be able to add records to the Booking table.
* User Nikki must be able to remove records from the Booking table.
* User Phil is no longer allowed to add data to the Guest table.
* User Phil is no longer allowed to delete records from the Guest table. Assume usernames
of employees Nikki and Phil are nikki and phil respectively.

```
GRANT INSERT PRIVILEGES ON Booking To nikki
GRANT DELETE PRIVILEGES ON Booking To nikki
REVOKE INSERT PRIVILEGES ON Guest To phil
REVOKE DELETE PRIVILEGES ON Guest To phil
```
