# **Welcome to SQL Server Collection's of Hadiuzzaman**
- Here I will collect all types of complex queries.
- which was used for my work purposes.

## My SQL GURU
```
- Khan Mohammad Sohel Zaman Bhai
- Nazir Ali Bhai
- Shahadat Bhai
- Forhad Bhai
- Rubel Bhai
- Rahat Bhai
- Omar Faruk Shamim Bhai
- Ahsan Kabir
```

## My First Qyery
```
Select * from tblProduct
Learn from Nazir Ali Bhai (Ex Southtech Colleague)
```

# The most useful website for SQL and Others
- https://www.w3schools.com/sql/sql_intro.asp
- https://www.w3resource.com/sql-exercises/
- https://sqlskull.com/2020/03/15/sql-server-lead-function/
- https://devsonket.com/
- 
###### You Can Download Some Book From Here
- https://www.db-book.com/db6/practice-exer-dir/index.html
###### TOP-70 MOST IMPORTANT SQL QUERIES IN 2022
- https://bytescout.com/blog/20-important-sql-queries.html?utm_referer=https%3A%2F%2Fwww.google.com%2F#35
###### Join over 16 million developers in solving code challenges on (Best Practice Site for SQL)
- https://www.hackerrank.com/domains/sql


## Find empty tables in SQL Server database
```
Select	schema_name(tab.schema_id) + '.' + tab.name as [table]
		From sys.tables tab
		Inner join sys.partitions part
		On tab.object_id = part.object_id

Where		part.index_id IN (1, 0)	-- 0 - table without PK, 1 table with PK
Group By	schema_name(tab.schema_id) + '.' + tab.name
Having		sum(part.rows) = 0
Order By	[table]
```
# Row to Colomn SQL Pivot
```
SELECT   ShopCOde, Pro_Code, 1 as AreaCode_1, 2 as AreaCode_2 FROM   [dbo].[tblPvot] 
PIVOT (SUM(Stock) FOR [AreaCode] IN ([1], [2])) AS P

Select ShopCode,Pro_Code,AreaCode,Stock from tblPvot

```

# Disable Relation 
```
-- Disable the constraints on a table called tableName:
ALTER TABLE tableName NOCHECK CONSTRAINT ALL

-- Re-enable the constraints on a table called tableName:
ALTER TABLE tableName WITH CHECK CHECK CONSTRAINT ALL
```

# RESTORE DATABASE DBNAME
```
From Disk='G:\25-06-2020\Backup\Backup.bak'
WITH	Move 'POS_data'		to 'G:\POS_AUDIT.mdf',
		Move 'POS_data2'	to 'G:\POS_AUDIT_2.mdf',
		Move 'POS_data1'	to 'G:\POS_AUDIT_1.mdf',
		Move 'POS_log'		to 'G:\POS_AUDIT_Log.ldf',
		Password='abc'
		--Password='',
		--Password=''
```



# SQL User disable or user sleep 
```
Select Session_id
From Sys.dm.exec_session where login_name='prince'
```

# To get total number of columns in a table in sql
```
SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.Columns where TABLE_NAME = 'YourTableName'
```
# Current Month Last Last Date and To get the last day of the previous month:
```
--Current Month Last Last Date
DECLARE @test DATETIME
SET @test = GETDATE()  -- or any other date
SELECT DATEADD(month, ((YEAR(@test) - 1900) * 12) + MONTH(@test), -1)

--To get the last day of the previous month:
SELECT DATEADD(DAY, -(DAY(GETDATE())), GETDATE())

```

# Create Indexing or index
```
--For Indexing or index
Create INDEX IX_SalesReceiptNO
ON [dbo].[tblSalesTransactionDetails]
([ReceiptNo] ASC)

###### 2 (two) Cloumn Indexing or index
--For Indexing or index
Create INDEX IX_SalesReceiptNO
ON [dbo].[tblSalesTransactionDetails]
([ReceiptNo] ASC)
or
Create INDEX IX_SalesReceiptNO
ON [dbo].[tblSalesTransactionDetails]
(ReceiptNo ASC, Barcode asc)

```



# Find Date Month year 
```
--Find Date Month year 
Select FDate, FMonth, FYear from #FDateTimeYear

CREATE TABLE #FDateTimeYear
( [FDate] [varchar](2) NOT NULL,
[FMonth] [varchar](3) NULL,
[FYear] [varchar](4) NULL )

Insert Into #FDateTimeYear
SELECT DATENAME(D,GETDATE()) 'FDate',
Convert(char(3), GetDate(), 0) as FMonth , DATENAME(YEAR,GETDATE()) 'FYear'

Select FDate + '-' + FMonth + '-' as PDateMonth,Fyear into #FromDate From #FDateTimeYear
Select PDateMonth+FYear from #FromDate
Select PDateMonth+PYear from #Todate

Select FDate + '-' + FMonth + '-' as PDateMonth,
Case When Fyear >0 Then Fyear-1 END AS PYear
Into #Todate From #FDateTimeYear

# DateMonthYear
--DateMonthYear
SELECT CONVERT(VARCHAR(19), SYSDATETIME(), 120)
SELECT DATEADD(dd, -1, DATEADD(yy, DATEDIFF(yy, 0, GETDATE()), 0))
```

# how to display space first letter capital in sql server
# After Space Letter Capital First Letter in capital, Upper and Lower font style
```
Select stuff((
       select ' '+upper(left(T3.V, 1))+lower(stuff(T3.V, 1, 1, ''))
       from (select cast(replace((select CustomerName as '*' for xml path('')), ' ', '<X/>') as xml).query('.')) as T1(X)
         cross apply T1.X.nodes('text()') as T2(X)
         cross apply (select T2.X.value('.', 'varchar(30)')) as T3(V)
       for xml path(''), type
       ).value('text()[1]', 'varchar(30)'), 1, 1, '')  from #CustomerBalance_2020 
```

# Auto Row Count Row Number
```
--Auto Row Count Row Number
Select ROW_NUMBER () Over(Order by UserID ) as SLN, UserID from tblUsers
```
# Varchar Date Month Year
```
--Varchar Date Month Year
SELECT CONVERT(VARCHAR(19), (SELECT DAY(GETDATE())), 103) as DAY 
SELECT CONVERT(VARCHAR(19), (SELECT UPPER(Convert(char(3), GetDate(), 0))), 103) as MONTH 
SELECT CONVERT(VARCHAR(19), (SELECT YEAR(GETDATE())), 103) as YEAR
Create Table #DayMonthYear
(Day varchar (20), Month varchar (20), Year varchar (20), LastYear varchar (20))
Insert Into #DayMonthYear
SELECT CONVERT(VARCHAR(19), (SELECT DAY(GETDATE())), 103) as DAY ,
CONVERT(VARCHAR(19), (SELECT UPPER(Convert(char(3), GetDate(), 0))), 103) as MONTH ,
CONVERT(VARCHAR(19), (SELECT YEAR(GETDATE())), 103) as YEAR,
Year(Getdate())- 1 as LastYear
--------------------------------------------------------------------------------------------
Select Day + '-' + Month + '-' + Year as FromDate into #FromDate from #DayMonthYear
Select Day + '-' + Month + '-' + LastYear as ToDate Into #ToDate from #DayMonthYear

Drop table #FromDate
Drop table #DayMonthYear
```


# IF EXISTS

```
Declare @Return as Varchar (MAX)
	IF EXISTS (Select * from tblUsers where UserID='ADMN2')
	
		Begin
		Select @Return		=	'FOUND'
		END
			ELSE
			BEGIN
			Select @Return	=	'NotFound'	--IF not Found Then Insert LastDateOFPreviousMonth
			END
			
			Select @Return as Result   --Forhad Bhai KKHO
---------------------------------------------------------------------------------------------------------
IF EXISTS (Select * from #Result where Prod_CodeInsert_OK_To_tblRequisition like '01%')

BEGIN
        PRINT 'Found'
END
ELSE
		BEGIN
		PRINT	'NotFound'	--IF not Found Then Inser LastDateOFPreviousMonth
		END
```


# Bulk Insert From Excel to SQL Using Query
###### Before Bulk Insert Need to Install and Run the Below Query
```
---------------------------
--SQL Ad Hoc Access to OLE DB
EXEC sp_configure 'show advanced options', 1
RECONFIGURE
GO
EXEC sp_configure 'ad hoc distributed queries', 1
RECONFIGURE
GO
-----------------------------------------------------------------------------
"C:\Users\abluhadi\Documents\Downloade\AccessDatabaseEngine_x64.exe" /passive
https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255
```
###### Bulk Insert From Excel to SQL Using Query
```
--Bulk Insert From Excel to SQL Using Query
Create table tblemployees
(ID int primary key identity(1,1),
Name varchar (500),
Type varchar (500),
F_Name varchar (500),
M_Name varchar (500),
S_Name varchar (500),
District varchar (500),
Country varchar (500),
Thana varchar (500),
Roll varchar (500),
Salary decimal)

Select * from tblemployees

BULK INSERT tblemployees
FROM 'C:\DATA\PERSONAL\TESTWORK\TBLEMPLOYEES.CSV'
WITH	(ROWTERMINATOR ='\n',
		FIELDTERMINATOR=',',
		FIRSTROW=2		)

Delete from tblemployees
Drop table tblemployees

--Bulk Insert From Excel to SQL Using Query
```


# Query Execute or Run from Table Data
```
--Query Execute Run from Table Data

DECLARE     @query		nVARCHAR(MAX) = '';

SET @query =(SELECT LastName FROM   Persons)

-- execute dynamic query
EXECUTE sp_executesql @query;

SELECT LastName FROM   Persons
```



# Quickest Way to Run the Same Query Multiple Times in SQL Server

```
--Quickest Way to Run the Same Query Multiple Times in SQL Server

CREATE TABLE LoopTest
(    LoopTestId uniqueidentifier NOT NULL DEFAULT NEWID(),
    InsertDate datetime2(7) NOT NULL DEFAULT GETDATE() );
GO
INSERT LoopTest (LoopTestId, InsertDate)
VALUES (DEFAULT, DEFAULT);
GO 20

Select * from LoopTest
```

# Multiple Date & Time Formate 

| SerialNo | Query | Format | Sample |
| --- | --- | --- | --- |
| 01 | select convert(varchar, getdate(), 1) | mm/dd/yy | 12/30/06 |
| 02 | select convert(varchar, getdate(), 2) | yy.mm.dd | 06.12.30 |
| 03 | select convert(varchar, getdate(), 3) | dd/mm/yy | 30/12/06 |
| 04 | select convert(varchar, getdate(), 4) | dd.mm.yy | 30.12.06 |
| 05| select convert(varchar, getdate(), 5) | dd-mm-yy | 39081 |
| 06| select convert(varchar, getdate(), 6) | dd-Mon-yy | 39081 |
| 07| select convert(varchar, getdate(), 7) | Mon dd, yy | 39081 |
| 08| select convert(varchar, getdate(), 10) | mm-dd-yy | 12-30-06 |
| 09| select convert(varchar, getdate(), 11) | yy/mm/dd | 11298 |
| 10| select convert(varchar, getdate(), 12) | yymmdd | 61230 |
| 11| select convert(varchar, getdate(), 23) | yyyy-mm-dd | 39081 |
| 12| select convert(varchar, getdate(), 101) | mm/dd/yyyy | 12/30/2006 |
| 13| select convert(varchar, getdate(), 102) | yyyy.mm.dd | 2006.12.30 |
| 14| select convert(varchar, getdate(), 103) | dd/mm/yyyy | 39081 |
| 15| select convert(varchar, getdate(), 104) | dd.mm.yyyy | 30.12.2006 |
| 16| select convert(varchar, getdate(), 105) | dd-mm-yyyy | 39081 |
| 17| select convert(varchar, getdate(), 106) | dd Mon yyyy | 39081 |
| 18| select convert(varchar, getdate(), 107) | Mon dd, yyyy | 39081 |
| 19| select convert(varchar, getdate(), 110) | mm-dd-yyyy | 12-30-2006 |
| 20| select convert(varchar, getdate(), 111) | yyyy/mm/dd | 39081 |
| 21| select convert(varchar, getdate(), 112) | yyyymmdd | 20061230 |
| 22| select convert(varchar, getdate(), 8) | hh:mm:ss | 0.0270138888888889 |
| 23| select convert(varchar, getdate(), 14) | hh:mm:ss:nnn | 00:38:54:840 |
| 24| select convert(varchar, getdate(), 24) | hh:mm:ss | 0.0270138888888889 |
| 25| select convert(varchar, getdate(), 108) | hh:mm:ss | 0.0270138888888889 |
| 26| select convert(varchar, getdate(), 114) | hh:mm:ss:nnn | 00:38:54:840 |
| 27| select convert(varchar, getdate(), 0) | Mon dd yyyy hh:mm AM/PM | Dec 30 2006 12:38AM |
| 28| select convert(varchar, getdate(), 9) | Mon dd yyyy hh:mm:ss:nnn AM/PM | Dec 30 2006 12:38:54:840AM |
| 29| select convert(varchar, getdate(), 13) | dd Mon yyyy hh:mm:ss:nnn AM/PM | 30 Dec 2006 00:38:54:840AM |
| 30| select convert(varchar, getdate(), 20) | yyyy-mm-dd hh:mm:ss | 39081.0270138889 |
| 31| select convert(varchar, getdate(), 21) | yyyy-mm-dd hh:mm:ss:nnn | 39081.0270236111 |
| 32| select convert(varchar, getdate(), 22) | mm/dd/yy hh:mm:ss AM/PM | 12/30/06 12:38:54 AM |
| 33| select convert(varchar, getdate(), 25) | yyyy-mm-dd hh:mm:ss:nnn | 39081.0270236111 |
| 34| select convert(varchar, getdate(), 100) | Mon dd yyyy hh:mm AM/PM | Dec 30 2006 12:38AM |
| 35| select convert(varchar, getdate(), 109) | Mon dd yyyy hh:mm:ss:nnn AM/PM | Dec 30 2006 12:38:54:840AM |
| 36| select convert(varchar, getdate(), 113) | dd Mon yyyy hh:mm:ss:nnn | 30 Dec 2006 00:38:54:840 |
| 37| select convert(varchar, getdate(), 120) | yyyy-mm-dd hh:mm:ss | 39081.0270138889 |
| 38| select convert(varchar, getdate(), 121) | yyyy-mm-dd hh:mm:ss:nnn | 39081.0270236111 |

###### Convert Date time to Number
```
select replace(convert(varchar, getdate(),101),'/','')		
select replace(convert(varchar, getdate(),101),'/','') + replace(convert(varchar, getdate(),108),':','')		
```


# Plan_Cursor or FETCH
```
DECLARE @id varchar(50) ,@maxDayQty numeric(14,2), @startDate numeric(14), @raminingQty numeric(14)
DECLARE @tempDate numeric(14)

DECLARE plan_cursor CURSOR FOR
SELECT ID, MaxDayQty, StartDate, RemainingQty FROM tblPlan where RemainingQty > 0 --AND  ID = '103'
order by ID;

OPEN plan_cursor

FETCH NEXT FROM plan_cursor
INTO @id,@maxDayQty,@startDate,@raminingQty

SET @tempDate = @startDate

WHILE @@FETCH_STATUS = 0
BEGIN
	
	WHILE (@raminingQty >0)
		BEGIN
				IF((@raminingQty - @maxDayQty) > 0)
					BEGIN
						Insert into tblDateWiseQuantity
						Select @id AS ID ,@tempDate as ProductionDate, @maxDayQty AS MaxDayQty 

						update tblPlan set RemainingQty = RemainingQty - @maxDayQty,EndDate = @tempDate
						where ID = @id
					END
				ELSE
					BEGIN
						Insert into tblDateWiseQuantity
						Select @id AS ID ,@tempDate as ProductionDate, @raminingQty AS MaxDayQty
						update tblPlan set RemainingQty = 0,EndDate = @tempDate  
						where ID = @id
					END

				SET @tempDate = @tempDate + 1;
				SET @raminingQty = (select RemainingQty from tblPlan where ID = @id)

		END
	

    FETCH NEXT FROM plan_cursor
	INTO @id,@maxDayQty,@startDate,@raminingQty
	SET @tempDate = @startDate
END
CLOSE plan_cursor;
DEALLOCATE plan_cursor;
```


# Database Repair or DBCC CheckDB
```	
--1st Step  Delete All Temp Table
USE [master]

Alter Database [DBNAME] Set Single_USer 
OR
Alter Database [DBNAME] Set Single_USer with no_Wait
Go
		Select * from  sys.sysprocesses where dbid=DB_ID('DBNAME')
		Kill 52 --(request_Session)

DBCC CheckDB ('DBNAME', Repair_Rebuild)

Alter Database [DBNAME] Set Multi_USer

DBCC CHECKDB('DBNAME')

--2nd Step Table Check
	DBCC CHECKTable('tblProduct')
	DBCC CHECKTable('tblProductdetails')
	-- Need All Table al Slow Tables

--3rd Step Log File or may be Data Shrink

--4th Step Indexing  by Start a Task Weserd
```






















































<!-- For Color Tex
```diff
-dsglhfg
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@

--For Hyperlink
- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) `#f03c15`
---->
```






