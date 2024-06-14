Language used: C#
IDE: Visual Studio 2022
Database: SQL Server management studio 19
Architecture used: n-tier architecture
Design pattern: Abstract Factory Design Pattern
Application type: web application
-------------------------------------------------
DATABASE:
Database includes main 13 tables, 5 views (virtual tables)
4 triggers, CRUD queries for all 13 main tables.
* only 13 main tables are being used in web application
(views, triggers etc are for database requirements
as this project is for DBMS & SDA both courses).
--------------------------------------------------
DESIGN PATTERN:
Abstract factory design pettern is implemented because it 
was in reuirements of this project.
* AbstractFactory layer is only linked to work with 2 forms
'P_bookTickets' & 'Reciept'.
* If we want to remove design pattern, then we'll have to delete
layer named "AbstractFactory", and in 'reciprt.aspx' form 
changes will be made.
---------------------------------------------------
VIDEOS:
videos are in project video folder, they can be played with
windows media player.
Videos link is below, if you cant play them in windows
https://ayeshamishree.wistia.com/medias/kwj2jo65ne
https://ayeshamishree.wistia.com/medias/rd7ry2gmk3
----------------------------------------------------
UI: Debugging:
To run this project, unzip folder named "TRANSPORT MANAGEMENT SYSTEM",
inside folder there is file named "TMSystem1.sln", open it.
You'll have to shange connection string, to run.
connect database in project: tools>connect db> select db> advance, then
copy connection string, past it insted of connection string in following places.
"DAL.cs, Signup.aspx.cs, or may be in book ticket form (i don't remember)"
Run project form form 'PresentationLayer.aspx' present in layer/tier
'TMSSystem1'. (as it is home page)
Internet will be needed.
----------------------------------------------------
Download complete zip file from this link
https://drive.google.com/file/d/1I-dDV_thE6A7osPs_6K7Sr2hNqEYiYFv/view?usp=sharing

