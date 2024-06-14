--TRANSPORT MANAGEMNET SYSTEM DATABASE--

create database TMSdb;
use TMSdb;

create table DriverDetails(
    DriverID int identity(1,1) primary key,
    DriverName varchar(20) not null,
    DriverPhone varchar(20),
    Age numeric(3),
    Date_Of_Join date,
    constraint Age_check check(Age > 18)
);

create table loginTable(
    ID int identity(1,1) primary key,
    userName varchar(30),
    Password varchar(15) not null,
    Usertype varchar(10) not null
);

create table Passenger(
	pasID INT identity(1,1) PRIMARY KEY CHECK (pasID > 0),
	PassengerName varchar(50),
	PassengerGender varchar(7),
    Age int constraint age2_chk check(Age > 5)
);

create table RouteDetails(
RouteID int identity(1,1) primary key constraint Routeid4_chk check(RouteID>0),
RouteName varchar(30) not null,
Source varchar(30) not null,
Destination varchar(30) not null
);

create table AgencyDetails(
AgencyName varchar(35) primary key not null,
AgencyAddress varchar(50) not null,
AgencyPhone varchar(20)
);

create table Admin(
    AdminID int primary key not null,
    AdminName varchar(255),
	AdminPhone varchar(20),
	AdminGender varchar(10),
	AdminAge int,
	AgencyName varchar(35) foreign key references AgencyDetails(AgencyName)
);

create table BusInfo(
    BusRegnNo varchar(15) primary key not null,
	BusName varchar(20),
    TotalSeats integer default 40,
    Latitude numeric(17,10),
    Longitude numeric(17,10),
	AgencyName varchar(35) foreign key references AgencyDetails(AgencyName)
);

create table TimeForTravel(
TravelTime NUMERIC(10) CONSTRAINT TravelTime_cons CHECK (TravelTime >= 6 AND TravelTime <= 12),
StartTime numeric(4,2) constraint Start1_check check(StartTime>=0 and StartTime<24),
EndTime numeric(4,2) constraint End1_chk check(EndTime>=0 and EndTime<24),
primary key(TravelTime,StartTime)
);

CREATE TABLE Ticket (
    TicketPNR INT IDENTITY(1,1) PRIMARY KEY NOT NULL,
    BusRegnNo VARCHAR(15) FOREIGN KEY REFERENCES BusInfo(BusRegnNo),
    SeatNo INT,
    RowNo INT,
    BookingDate DATE,
    TravelDate DATE, 
    TravelTime NUMERIC(10) CONSTRAINT TravelTime_con CHECK (TravelTime >= 6 AND TravelTime <= 12),
    FOREIGN KEY (BusRegnNo, SeatNo, RowNo) REFERENCES SeatInfo(BusRegnNo, SeatNo, RowNo)
);


CREATE TABLE SeatsBooked (
    TicketPNR INT,
    BusRegnNo VARCHAR(15),
    SeatNo INT,
    RowNo INT,
    FOREIGN KEY (TicketPNR) REFERENCES Ticket(TicketPNR),
    FOREIGN KEY (BusRegnNo, SeatNo, RowNo) REFERENCES SeatInfo(BusRegnNo, SeatNo, RowNo)
);

CREATE TABLE SeatInfo (
    BusRegnNo VARCHAR(15),
    SeatNo INT CHECK (SeatNo > 0),
    RowNo INT CHECK (RowNo > 0),
    Status VARCHAR(10) DEFAULT 'unbooked',
    PRIMARY KEY (BusRegnNo, SeatNo, RowNo) 
);


create table BusStops(
RouteID integer not null constraint Routeid_chk check(RouteID>0),
IntermediateStops varchar(20) not null,
StopNumber integer not null constraint Stop_num check(StopNumber>0)
);

CREATE TABLE BusSchedule (
    BusRegnNo VARCHAR(15) NOT NULL,
    RouteID INT,
    DriverID INT,
    StartTime NUMERIC(4,2),
    Fare INTEGER CONSTRAINT fare_chk CHECK (Fare > 20),
    ReservedSeats INTEGER DEFAULT 0,
    PRIMARY KEY (RouteID, DriverID, StartTime),
    FOREIGN KEY (BusRegnNo) REFERENCES BusInfo(BusRegnNo),
    FOREIGN KEY (RouteID) REFERENCES RouteDetails(RouteID),
    FOREIGN KEY (DriverID) REFERENCES DriverDetails(DriverID),
    CONSTRAINT Start_chk CHECK (StartTime >= 6 AND StartTime <= 12)
);

-----------------------------------------
-- 13 tables created --------------------

INSERT INTO DriverDetails (DriverName, DriverPhone, Age, Date_Of_Join) 
VALUES ('Ali Hamza', '+92 123456789', 35, '2023-05-10'),
       ('Hamza AH', '+92 123456789', 28, '2023-03-15'),
       ('M Faiz', '+92 123456789', 40, '2023-01-20');

INSERT INTO loginTable (userName, Password, Usertype) 
VALUES ('alee12', 'password123', 'passenger'),
       ('sarah7', 'password456', 'passenger'),
       ('michaelJ', 'password789', 'passenger');

INSERT INTO Passenger (PassengerName, PassengerGender, Age)
VALUES ('Sarah H', 'Female', 25),
       ('Michael J', 'Male', 30),
       ('Alee K', 'Male', 20);

INSERT INTO RouteDetails (RouteName, Source, Destination)
VALUES ('RouteA', 'Johar', 'Nazimabad'),
       ('RouteB', 'Gulshan', 'PECHS'),
       ('RouteC', 'Orangi', 'Hub');

INSERT INTO AgencyDetails (AgencyName, AgencyAddress, AgencyPhone)
VALUES ('Karachi Transport', '123 Main Street, Karachi', '0321-1234567')


INSERT INTO Admin (AdminID, AdminName, AdminPhone, AdminGender, AdminAge, AgencyName)
VALUES (1, 'Ahmed', '9998887777', 'Male', 45, 'Karachi Transport'),
       (3, 'Maria', '7776665555', 'Female', 50, 'Karachi Transport');

INSERT INTO BusInfo (BusRegnNo, BusName, Latitude, Longitude, AgencyName)
VALUES ('KHI123', 'BusA', 24.8607, 67.0011, 'Karachi Transport'),
       ('KHI456', 'BusB', 24.8615, 67.0099, 'Karachi Transport'),
       ('KHI789', 'BusC', 24.8610, 67.0035, 'Karachi Transport');

INSERT INTO TimeForTravel (TravelTime, StartTime, EndTime)
VALUES (6, 8.00, 10.00),
       (12, 9.00, 11.00),
       (7, 10.00, 12.00);

INSERT INTO BusStops (RouteID, IntermediateStops, StopNumber)
VALUES (1, 'Stop1', 1),
       (1, 'Stop2', 2),
       (1, 'Stop3', 3);

INSERT INTO BusSchedule (BusRegnNo, RouteID, DriverID, StartTime, Fare)
VALUES ('KHI123', 1, 1, 8.00, 500),
       ('KHI456', 2, 2, 9.00, 600),
       ('KHI789', 3, 3, 10.00, 700);


INSERT INTO SeatInfo (BusRegnNo, SeatNo, RowNo, Status)
VALUES ('KHI123', 1, 1, 'unbooked'),
       ('KHI123', 2, 1, 'unbooked'),
       ('KHI123', 3, 2, 'unbooked');

INSERT INTO SeatsBooked (TicketPNR, BusRegnNo, SeatNo, RowNo)
VALUES (1, 'KHI123', 1, 1),
       (2, 'KHI123', 2, 1),
       (3, 'KHI123', 3, 2);

INSERT INTO Ticket (BusRegnNo, SeatNo, RowNo, BookingDate, TravelDate, TravelTime)
VALUES ('KHI123', 1, 1, '2024-05-11', '2024-05-15', 6),
       ('KHI123', 2, 1, '2024-05-11', '2024-05-15', 6),
       ('KHI123', 3, 2, '2024-05-11', '2024-05-15', 6);

---------------------------------------
----filled all tables


-- Update status of booked seats in SeatsBooked table
UPDATE SeatInfo
SET Status = 'booked'
WHERE EXISTS (
    SELECT 1
    FROM SeatsBooked sb
    JOIN Ticket t ON sb.TicketPNR = t.TicketPNR
    WHERE SeatInfo.BusRegnNo = sb.BusRegnNo
    AND SeatInfo.SeatNo = sb.SeatNo
    AND SeatInfo.RowNo = sb.RowNo
);

select * from Admin
select * from AgencyDetails
select * from BusInfo
select * from BusSchedule
select * from BusStops
select * from DriverDetails
select * from Passenger
select * from RouteDetails
select * from TimeForTravel
select * from Seatinfo
select * from SeatsBooked
select * from Ticket


--------------------------------------------------------------------------
-------------------CRUD QUERIES FOR ALL TABLES----------------------------

--------------DriverDetails---------------------

BEGIN TRANSACTION;
DECLARE @DriverName VARCHAR(20), @DriverPhone VARCHAR(20), @Age NUMERIC(3), @Date_Of_Join DATE;
SET @DriverName = 'New Driver';
SET @DriverPhone = '123456789';
SET @Age = 30;
SET @Date_Of_Join = '2024-01-01';

INSERT INTO DriverDetails (DriverName, DriverPhone, Age, Date_Of_Join)
VALUES (@DriverName, @DriverPhone, @Age, @Date_Of_Join);

COMMIT TRANSACTION;
--------------------------------------
DECLARE @DriverID INT;
SET @DriverID = 1;

SELECT * FROM DriverDetails
WHERE DriverID = @DriverID;
--------------------------------------
BEGIN TRANSACTION;
DECLARE @DriverID INT, @DriverName VARCHAR(20), @DriverPhone VARCHAR(20), @Age NUMERIC(3), @Date_Of_Join DATE;
SET @DriverID = 1;
SET @DriverName = 'Updated Driver';
SET @DriverPhone = '987654321';
SET @Age = 35;
SET @Date_Of_Join = '2024-01-01';

UPDATE DriverDetails
SET DriverName = @DriverName, DriverPhone = @DriverPhone, Age = @Age, Date_Of_Join = @Date_Of_Join
WHERE DriverID = @DriverID;

COMMIT TRANSACTION;
-------------------------------------
BEGIN TRANSACTION;
DECLARE @DriverID INT;
SET @DriverID = 1;

DELETE FROM DriverDetails
WHERE DriverID = @DriverID;

COMMIT TRANSACTION;


--------------loginTable------------------------
BEGIN TRANSACTION;
DECLARE @userName VARCHAR(30), @Password VARCHAR(15), @Usertype VARCHAR(10);
SET @userName = 'newUser';
SET @Password = 'newPassword';
SET @Usertype = 'passenger';

INSERT INTO loginTable (userName, Password, Usertype)
VALUES (@userName, @Password, @Usertype);

COMMIT TRANSACTION;
------------------------------------
DECLARE @ID INT;
SET @ID = 1;

SELECT * FROM loginTable
WHERE ID = @ID;
------------------------------------
BEGIN TRANSACTION;
DECLARE @ID INT, @userName VARCHAR(30), @Password VARCHAR(15), @Usertype VARCHAR(10);
SET @ID = 1;
SET @userName = 'updatedUser';
SET @Password = 'updatedPassword';
SET @Usertype = 'admin';

UPDATE loginTable
SET userName = @userName, Password = @Password, Usertype = @Usertype
WHERE ID = @ID;

COMMIT TRANSACTION;
------------------------------------
BEGIN TRANSACTION;
DECLARE @ID INT;
SET @ID = 1;

DELETE FROM loginTable
WHERE ID = @ID;

COMMIT TRANSACTION;

--------------Passenger-------------------------
BEGIN TRANSACTION;
DECLARE @PassengerName VARCHAR(50), @PassengerGender VARCHAR(7), @Age INT;
SET @PassengerName = 'New Passenger';
SET @PassengerGender = 'Female';
SET @Age = 25;

INSERT INTO Passenger (PassengerName, PassengerGender, Age)
VALUES (@PassengerName, @PassengerGender, @Age);

COMMIT TRANSACTION;
------------------------------
DECLARE @pasID INT;
SET @pasID = 1;

SELECT * FROM Passenger
WHERE pasID = @pasID;
------------------------------
BEGIN TRANSACTION;
DECLARE @pasID INT, @PassengerName VARCHAR(50), @PassengerGender VARCHAR(7), @Age INT;
SET @pasID = 1;
SET @PassengerName = 'Updated Passenger';
SET @PassengerGender = 'Male';
SET @Age = 30;

UPDATE Passenger
SET PassengerName = @PassengerName, PassengerGender = @PassengerGender, Age = @Age
WHERE pasID = @pasID;

COMMIT TRANSACTION;
---------------------------------
BEGIN TRANSACTION;
DECLARE @pasID INT;
SET @pasID = 1;

DELETE FROM Passenger
WHERE pasID = @pasID;

COMMIT TRANSACTION;

--------------RouteDetails----------------------
BEGIN TRANSACTION;
DECLARE @RouteName VARCHAR(30), @Source VARCHAR(30), @Destination VARCHAR(30);
SET @RouteName = 'New Route';
SET @Source = 'Source City';
SET @Destination = 'Destination City';

INSERT INTO RouteDetails (RouteName, Source, Destination)
VALUES (@RouteName, @Source, @Destination);

COMMIT TRANSACTION;
-------------------------------
DECLARE @RouteID INT;
SET @RouteID = 1;

SELECT * FROM RouteDetails
WHERE RouteID = @RouteID;
-------------------------------
BEGIN TRANSACTION;
DECLARE @RouteID INT, @RouteName VARCHAR(30), @Source VARCHAR(30), @Destination VARCHAR(30);
SET @RouteID = 1;
SET @RouteName = 'Updated Route';
SET @Source = 'Updated Source';
SET @Destination = 'Updated Destination';

UPDATE RouteDetails
SET RouteName = @RouteName, Source = @Source, Destination = @Destination
WHERE RouteID = @RouteID;

COMMIT TRANSACTION;
---------------------------------
BEGIN TRANSACTION;
DECLARE @RouteID INT;
SET @RouteID = 1;

DELETE FROM RouteDetails
WHERE RouteID = @RouteID;

COMMIT TRANSACTION;

--------------AgencyDetails---------------------
BEGIN TRANSACTION;
DECLARE @AgencyName VARCHAR(35), @AgencyAddress VARCHAR(50), @AgencyPhone VARCHAR(20);
SET @AgencyName = 'New Agency';
SET @AgencyAddress = 'New Address';
SET @AgencyPhone = '123456789';

INSERT INTO AgencyDetails (AgencyName, AgencyAddress, AgencyPhone)
VALUES (@AgencyName, @AgencyAddress, @AgencyPhone);

COMMIT TRANSACTION;
------------------------------
DECLARE @AgencyName VARCHAR(35);
SET @AgencyName = 'Karachi Transport';

SELECT * FROM AgencyDetails
WHERE AgencyName = @AgencyName;
---------------------------------
BEGIN TRANSACTION;
DECLARE @AgencyName VARCHAR(35), @AgencyAddress VARCHAR(50), @AgencyPhone VARCHAR(20);
SET @AgencyName = 'Karachi Transport';
SET @AgencyAddress = 'Updated Address';
SET @AgencyPhone = '987654321';

UPDATE AgencyDetails
SET AgencyAddress = @AgencyAddress, AgencyPhone = @AgencyPhone
WHERE AgencyName = @AgencyName;

COMMIT TRANSACTION;
-----------------------------------
BEGIN TRANSACTION;
DECLARE @AgencyName VARCHAR(35);
SET @AgencyName = 'Karachi Transport';

DELETE FROM AgencyDetails
WHERE AgencyName = @AgencyName;

COMMIT TRANSACTION;

--------------Admin-----------------------------
BEGIN TRANSACTION;
DECLARE @AdminID INT, @AdminName VARCHAR(255), @AdminPhone VARCHAR(20), @AdminGender VARCHAR(10), @AdminAge INT, @AgencyName VARCHAR(35);
SET @AdminID = 4;
SET @AdminName = 'New Admin';
SET @AdminPhone = '1122334455';
SET @AdminGender = 'Male';
SET @AdminAge = 40;
SET @AgencyName = 'Karachi Transport';

INSERT INTO Admin (AdminID, AdminName, AdminPhone, AdminGender, AdminAge, AgencyName)
VALUES (@AdminID, @AdminName, @AdminPhone, @AdminGender, @AdminAge, @AgencyName);

COMMIT TRANSACTION;
----------------------------------------------
DECLARE @AdminID INT;
SET @AdminID = 1;

SELECT * FROM Admin
WHERE AdminID = @AdminID;
-------------------------------------------
BEGIN TRANSACTION;
DECLARE @AdminID INT, @AdminName VARCHAR(255), @AdminPhone VARCHAR(20), @AdminGender VARCHAR(10), @AdminAge INT, @AgencyName VARCHAR(35);
SET @AdminID = 1;
SET @AdminName = 'Updated Admin';
SET @AdminPhone = '9988776655';
SET @AdminGender = 'Female';
SET @AdminAge = 45;
SET @AgencyName = 'Karachi Transport';

UPDATE Admin
SET AdminName = @AdminName, AdminPhone = @AdminPhone, AdminGender = @AdminGender, AdminAge = @AdminAge, AgencyName = @AgencyName
WHERE AdminID = @AdminID;

COMMIT TRANSACTION;
-----------------------------------------
BEGIN TRANSACTION;
DECLARE @AdminID INT;
SET @AdminID = 1;

DELETE FROM Admin
WHERE AdminID = @AdminID;

COMMIT TRANSACTION;

--------------BusInfo---------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @BusName VARCHAR(20), @Latitude NUMERIC(17,10), @Longitude NUMERIC(17,10), @AgencyName VARCHAR(35);
SET @BusRegnNo = 'NEW123';
SET @BusName = 'New Bus';
SET @Latitude = 24.8607;
SET @Longitude = 67.0011;
SET @AgencyName = 'Karachi Transport';

INSERT INTO BusInfo (BusRegnNo, BusName, Latitude, Longitude, AgencyName)
VALUES (@BusRegnNo, @BusName, @Latitude, @Longitude, @AgencyName);

COMMIT TRANSACTION;
----------------------------------------
DECLARE @BusRegnNo VARCHAR(15);
SET @BusRegnNo = 'KHI123';

SELECT * FROM BusInfo
WHERE BusRegnNo = @BusRegnNo;
---------------------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @BusName VARCHAR(20), @Latitude NUMERIC(17,10), @Longitude NUMERIC(17,10), @AgencyName VARCHAR(35);
SET @BusRegnNo = 'KHI123';
SET @BusName = 'Updated Bus';
SET @Latitude = 24.8610;
SET @Longitude = 67.0020;
SET @AgencyName = 'Karachi Transport';

UPDATE BusInfo
SET BusName = @BusName, Latitude = @Latitude, Longitude = @Longitude, AgencyName = @AgencyName
WHERE BusRegnNo = @BusRegnNo;

COMMIT TRANSACTION;
-----------------------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15);
SET @BusRegnNo = 'KHI123';

DELETE FROM BusInfo
WHERE BusRegnNo = @BusRegnNo;

COMMIT TRANSACTION;

--------------TimeForTravel---------------------
BEGIN TRANSACTION;
DECLARE @TravelTime NUMERIC(10), @StartTime NUMERIC(4,2), @EndTime NUMERIC(4,2);
SET @TravelTime = 8;
SET @StartTime = 6.00;
SET @EndTime = 8.00;

INSERT INTO TimeForTravel (TravelTime, StartTime, EndTime)
VALUES (@TravelTime, @StartTime, @EndTime);

COMMIT TRANSACTION;
----------------------------------
DECLARE @TravelTime NUMERIC(10), @StartTime NUMERIC(4,2);
SET @TravelTime = 6;
SET @StartTime = 8.00;

SELECT * FROM TimeForTravel
WHERE TravelTime = @TravelTime AND StartTime = @StartTime;
-------------------------------
BEGIN TRANSACTION;
DECLARE @TravelTime NUMERIC(10), @StartTime NUMERIC(4,2), @EndTime NUMERIC(4,2);
SET @TravelTime = 6;
SET @StartTime = 8.00;
SET @EndTime = 10.00;

UPDATE TimeForTravel
SET EndTime = @EndTime
WHERE TravelTime = @TravelTime AND StartTime = @StartTime;

COMMIT TRANSACTION;
------------------------------
BEGIN TRANSACTION;
DECLARE @TravelTime NUMERIC(10), @StartTime NUMERIC(4,2);
SET @TravelTime = 6;
SET @StartTime = 8.00;

DELETE FROM TimeForTravel
WHERE TravelTime = @TravelTime AND StartTime = @StartTime;

COMMIT TRANSACTION;

--------------Ticket----------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT, @BookingDate DATE, @TravelDate DATE, @TravelTime NUMERIC(10);
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 4;
SET @RowNo = 1;
SET @BookingDate = '2024-06-01';
SET @TravelDate = '2024-06-15';
SET @TravelTime = 6;

INSERT INTO Ticket (BusRegnNo, SeatNo, RowNo, BookingDate, TravelDate, TravelTime)
VALUES (@BusRegnNo, @SeatNo, @RowNo, @BookingDate, @TravelDate, @TravelTime);

COMMIT TRANSACTION;
-------------------------------------
DECLARE @TicketPNR INT;
SET @TicketPNR = 1;

SELECT * FROM Ticket
WHERE TicketPNR = @TicketPNR;
-------------------------------------
BEGIN TRANSACTION;
DECLARE @TicketPNR INT, @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT, @BookingDate DATE, @TravelDate DATE, @TravelTime NUMERIC(10);
SET @TicketPNR = 1;
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 4;
SET @RowNo = 1;
SET @BookingDate = '2024-06-01';
SET @TravelDate = '2024-06-15';
SET @TravelTime = 6;

UPDATE Ticket
SET BusRegnNo = @BusRegnNo, SeatNo = @SeatNo, RowNo = @RowNo, BookingDate = @BookingDate, TravelDate = @TravelDate, TravelTime = @TravelTime
WHERE TicketPNR = @TicketPNR;

COMMIT TRANSACTION;
-------------------------------------
BEGIN TRANSACTION;
DECLARE @TicketPNR INT;
SET @TicketPNR = 1;

DELETE FROM Ticket
WHERE TicketPNR = @TicketPNR;

COMMIT TRANSACTION;

--------------SeatsBooked-----------------------
BEGIN TRANSACTION;
DECLARE @TicketPNR INT, @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT;
SET @TicketPNR = 4;
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 4;
SET @RowNo = 1;

INSERT INTO SeatsBooked (TicketPNR, BusRegnNo, SeatNo, RowNo)
VALUES (@TicketPNR, @BusRegnNo, @SeatNo, @RowNo);

COMMIT TRANSACTION;
----------------------------------
DECLARE @TicketPNR INT;
SET @TicketPNR = 1;

SELECT * FROM SeatsBooked
WHERE TicketPNR = @TicketPNR;
----------------------------------
BEGIN TRANSACTION;
DECLARE @TicketPNR INT, @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT;
SET @TicketPNR = 1;
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 5;
SET @RowNo = 1;

UPDATE SeatsBooked
SET BusRegnNo = @BusRegnNo, SeatNo = @SeatNo, RowNo = @RowNo
WHERE TicketPNR = @TicketPNR;

COMMIT TRANSACTION;
-----------------------------------
BEGIN TRANSACTION;
DECLARE @TicketPNR INT;
SET @TicketPNR = 1;

DELETE FROM SeatsBooked
WHERE TicketPNR = @TicketPNR;

COMMIT TRANSACTION;

--------------SeatInfo--------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT, @Status VARCHAR(10);
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 6;
SET @RowNo = 1;
SET @Status = 'unbooked';

INSERT INTO SeatInfo (BusRegnNo, SeatNo, RowNo, Status)
VALUES (@BusRegnNo, @SeatNo, @RowNo, @Status);

COMMIT TRANSACTION;
------------------------------
DECLARE @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT;
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 1;
SET @RowNo = 1;

SELECT * FROM SeatInfo
WHERE BusRegnNo = @BusRegnNo AND SeatNo = @SeatNo AND RowNo = @RowNo;
------------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT, @Status VARCHAR(10);
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 1;
SET @RowNo = 1;
SET @Status = 'booked';

UPDATE SeatInfo
SET Status = @Status
WHERE BusRegnNo = @BusRegnNo AND SeatNo = @SeatNo AND RowNo = @RowNo;

COMMIT TRANSACTION;
------------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @SeatNo INT, @RowNo INT;
SET @BusRegnNo = 'KHI123';
SET @SeatNo = 1;
SET @RowNo = 1;

DELETE FROM SeatInfo
WHERE BusRegnNo = @BusRegnNo AND SeatNo = @SeatNo AND RowNo = @RowNo;

COMMIT TRANSACTION;

--------------BusStops--------------------------
BEGIN TRANSACTION;
DECLARE @RouteID INT, @IntermediateStops VARCHAR(20), @StopNumber INT;
SET @RouteID = 1;
SET @IntermediateStops = 'New Stop';
SET @StopNumber = 4;

INSERT INTO BusStops (RouteID, IntermediateStops, StopNumber)
VALUES (@RouteID, @IntermediateStops, @StopNumber);

COMMIT TRANSACTION;
------------------------------------
DECLARE @RouteID INT, @StopNumber INT;
SET @RouteID = 1;
SET @StopNumber = 1;

SELECT * FROM BusStops
WHERE RouteID = @RouteID AND StopNumber = @StopNumber;
-------------------------------------
BEGIN TRANSACTION;
DECLARE @RouteID INT, @IntermediateStops VARCHAR(20), @StopNumber INT;
SET @RouteID = 1;
SET @IntermediateStops = 'Updated Stop';
SET @StopNumber = 1;

UPDATE BusStops
SET IntermediateStops = @IntermediateStops
WHERE RouteID = @RouteID AND StopNumber = @StopNumber;

COMMIT TRANSACTION;
---------------------------------------
BEGIN TRANSACTION;
DECLARE @RouteID INT, @StopNumber INT;
SET @RouteID = 1;
SET @StopNumber = 1;

DELETE FROM BusStops
WHERE RouteID = @RouteID AND StopNumber = @StopNumber;

COMMIT TRANSACTION;

--------------BusSchedule-----------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @RouteID INT, @DriverID INT, @StartTime NUMERIC(4,2), @Fare INT, @ReservedSeats INT;
SET @BusRegnNo = 'KHI123';
SET @RouteID = 1;
SET @DriverID = 1;
SET @StartTime = 8.00;
SET @Fare = 500;
SET @ReservedSeats = 0;

INSERT INTO BusSchedule (BusRegnNo, RouteID, DriverID, StartTime, Fare, ReservedSeats)
VALUES (@BusRegnNo, @RouteID, @DriverID, @StartTime, @Fare, @ReservedSeats);

COMMIT TRANSACTION;
-----------------------------------------------
DECLARE @RouteID INT, @DriverID INT, @StartTime NUMERIC(4,2);
SET @RouteID = 1;
SET @DriverID = 1;
SET @StartTime = 8.00;

SELECT * FROM BusSchedule
WHERE RouteID = @RouteID AND DriverID = @DriverID AND StartTime = @StartTime;
-----------------------------------------------
BEGIN TRANSACTION;
DECLARE @BusRegnNo VARCHAR(15), @RouteID INT, @DriverID INT, @StartTime NUMERIC(4,2), @Fare INT, @ReservedSeats INT;
SET @BusRegnNo = 'KHI123';
SET @RouteID = 1;
SET @DriverID = 1;
SET @StartTime = 8.00;
SET @Fare = 600;
SET @ReservedSeats = 5;

UPDATE BusSchedule
SET Fare = @Fare, ReservedSeats = @ReservedSeats
WHERE RouteID = @RouteID AND DriverID = @DriverID AND StartTime = @StartTime;

COMMIT TRANSACTION;
-----------------------------------
BEGIN TRANSACTION;
DECLARE @RouteID INT, @DriverID INT, @StartTime NUMERIC(4,2);
SET @RouteID = 1;
SET @DriverID = 1;
SET @StartTime = 8.00;

DELETE FROM BusSchedule
WHERE RouteID = @RouteID AND DriverID = @DriverID AND StartTime = @StartTime;

COMMIT TRANSACTION;



--------------------------------------------------------------------------
------------------------------TRIGGERS--------------------------------------

------Trigger 1: Enforce Referential Integrity for BusSchedule-------
CREATE TRIGGER trg_BusSchedule_Insert
ON BusSchedule
AFTER INSERT
AS
BEGIN
    -- Check if the inserted RouteID exists in RouteDetails
    IF NOT EXISTS (SELECT 1 FROM RouteDetails WHERE RouteID = (SELECT RouteID FROM inserted))
    BEGIN
        RAISERROR ('RouteID does not exist in RouteDetails.', 16, 1);
        ROLLBACK TRANSACTION;
        RETURN;
    END

    -- Check if the inserted DriverID exists in DriverDetails
    IF NOT EXISTS (SELECT 1 FROM DriverDetails WHERE DriverID = (SELECT DriverID FROM inserted))
    BEGIN
        RAISERROR ('DriverID does not exist in DriverDetails.', 16, 1);
        ROLLBACK TRANSACTION;
        RETURN;
    END
END;

------------------Trigger 2: Log Changes in BusInfo------------------
-- Create a log table
CREATE TABLE BusInfoLog (
    LogID INT IDENTITY(1,1) PRIMARY KEY,
    BusRegnNo VARCHAR(15),
    ChangeType VARCHAR(10),
    ChangeTime DATETIME DEFAULT GETDATE(),
    OldBusName VARCHAR(20),
    NewBusName VARCHAR(20),
    OldLatitude NUMERIC(17,10),
    NewLatitude NUMERIC(17,10),
    OldLongitude NUMERIC(17,10),
    NewLongitude NUMERIC(17,10)
);

-- Create the trigger
CREATE TRIGGER trg_BusInfo_Update
ON BusInfo
AFTER UPDATE
AS
BEGIN
    INSERT INTO BusInfoLog (BusRegnNo, ChangeType, OldBusName, NewBusName, OldLatitude, NewLatitude, OldLongitude, NewLongitude)
    SELECT
        i.BusRegnNo,
        'UPDATE',
        d.BusName, i.BusName,
        d.Latitude, i.Latitude,
        d.Longitude, i.Longitude
    FROM
        inserted i
    JOIN
        deleted d ON i.BusRegnNo = d.BusRegnNo;
END;

------Trigger 3: Automatically Update Seat Status in SeatInfo--------
CREATE TRIGGER trg_SeatsBooked_Insert
ON SeatsBooked
AFTER INSERT
AS
BEGIN
    UPDATE SeatInfo
    SET Status = 'booked'
    WHERE EXISTS (
        SELECT 1
        FROM inserted i
        WHERE SeatInfo.BusRegnNo = i.BusRegnNo
        AND SeatInfo.SeatNo = i.SeatNo
        AND SeatInfo.RowNo = i.RowNo
    );
END;

----------Trigger 4: Ensure Age Constraint in Passenger--------------

CREATE TRIGGER trg_Passenger_Insert_Update
ON Passenger
AFTER INSERT, UPDATE
AS
BEGIN
    IF EXISTS (SELECT 1 FROM inserted WHERE Age < 6)
    BEGIN
        RAISERROR ('Passenger age must be greater than 5.', 16, 1);
        ROLLBACK TRANSACTION;
        RETURN;
    END
END;

--------------------------------------------------------------------------
------------------------------TEST CASES FOR TRIGGERS---------------------

BEGIN TRANSACTION;
INSERT INTO BusSchedule (BusRegnNo, RouteID, DriverID, StartTime, Fare, ReservedSeats)
VALUES ('KHI123', 999, 1, 8.00, 500, 0); -- This should fail if RouteID 999 does not exist
COMMIT TRANSACTION;
------------------------
BEGIN TRANSACTION;
UPDATE BusInfo
SET BusName = 'UpdatedBus', Latitude = 24.0000, Longitude = 67.0000
WHERE BusRegnNo = 'KHI123';
COMMIT TRANSACTION;

SELECT * FROM BusInfoLog; 
------------------------
BEGIN TRANSACTION;
INSERT INTO SeatsBooked (TicketPNR, BusRegnNo, SeatNo, RowNo)
VALUES (1, 'KHI123', 4, 1);
COMMIT TRANSACTION;

SELECT * FROM SeatInfo WHERE BusRegnNo = 'KHI123' AND SeatNo = 4 AND RowNo = 1; -- Status should be 'booked'
------------------------
BEGIN TRANSACTION;
INSERT INTO Passenger (PassengerName, PassengerGender, Age)
VALUES ('Test Passenger', 'Male', 5); -- This should fail due to age constraint
COMMIT TRANSACTION;



--------------------------------------------------------------------------
----------------------------------CREATING VIEWS -------------------------

----------View 1: Passenger Details with Their Booked Tickets-----------

CREATE VIEW PassengerTicketDetails AS
SELECT 
    p.pasID,
    p.PassengerName,
    p.PassengerGender,
    p.Age,
    t.TicketPNR,
    t.BusRegnNo,
    t.SeatNo,
    t.RowNo,
    t.BookingDate,
    t.TravelDate,
    t.TravelTime
FROM 
    Passenger p
JOIN 
    Ticket t ON p.pasID = t.TicketPNR;


----------View 2: Bus Schedule with Route and Driver Details-----------

CREATE VIEW BusScheduleDetails AS
SELECT 
    bs.BusRegnNo,
    bs.RouteID,
    r.RouteName,
    r.Source,
    r.Destination,
    bs.DriverID,
    d.DriverName,
    bs.StartTime,
    bs.Fare,
    bs.ReservedSeats
FROM 
    BusSchedule bs
JOIN 
    RouteDetails r ON bs.RouteID = r.RouteID
JOIN 
    DriverDetails d ON bs.DriverID = d.DriverID;


----------View 3: Active Buses with Their Locations-----------

CREATE VIEW ActiveBuses AS
SELECT 
    b.BusRegnNo,
    b.BusName,
    b.Latitude,
    b.Longitude,
    b.AgencyName
FROM 
    BusInfo b;

----------View 4: Agency Details with Their Buses-----------

CREATE VIEW AgencyBuses AS
SELECT 
    a.AgencyName,
    a.AgencyAddress,
    a.AgencyPhone,
    b.BusRegnNo,
    b.BusName
FROM 
    AgencyDetails a
JOIN 
    BusInfo b ON a.AgencyName = b.AgencyName;

----------View 5: Seat Availability for a Specific Bus-----------

CREATE VIEW SeatAvailability AS
SELECT 
    s.BusRegnNo,
    s.SeatNo,
    s.RowNo,
    s.Status
FROM 
    SeatInfo s;

-----------------------TESTING CREATED VIEWS----------------------

SELECT * FROM PassengerTicketDetails;
SELECT * FROM BusScheduleDetails;
SELECT * FROM ActiveBuses;
SELECT * FROM AgencyBuses;
SELECT * FROM SeatAvailability;

