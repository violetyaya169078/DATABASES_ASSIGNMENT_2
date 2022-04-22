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

<a id='Task3'></a>
### Task 3
Perform the following tasks.
* Write commands to insert one row of data in each of the Hotel database tables.
* Write a command to delete the row you inserted in the table Guest.
* Write a command to update the price of all rooms by 15%.

<a id='Task4'></a>
### Task 4
Perform the following tasks.
* Currently the database only contains a small number of records, however the data contained
within it is expected to grow significantly in the future. Creating indexes on commonly searched
columns is a way performance issues can be minimised. Write a command to create an index on
guestName of the Guest table.
* Write a command to create a view to list the information (hotelName, roomType and the total
number of rooms booked) of the hotels which are in Cairns.

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
