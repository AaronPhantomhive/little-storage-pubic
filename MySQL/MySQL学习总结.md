# MySQL学习总结

### 基本语法

#### CREATE table, insert statement

```mysql
CREATE TABLE Users (
		uid		INT			NOT NULL 	PRIMARY KEY		IDENTITY,
		un		VARCHAR(20)	NOT NULL	DEFALUT'',
		pw		VARCHAR(20)	NOT NULL	DEFALUT''
	)
	INSERT INTO Users(un, pw)
	VALUES ('tom','myPW')
	
SET IDENTITY_INSERT tablename ON
	INSERT INTO tablename(X1,X2,X3,X4)
	VALUES (1, 'X1', 'X2', 'X3'),
		   (3, 'X1', 'X2', 'X3')
SET IDENTITY_INSERT tablename OFF
```



#### SELECT FROM

```mysql
SELECT VendorCity, VendorState, [VendorCount] = COUNT(*)
FROM Vendors
```



#### count, group by, having, order by, where

```mysql
SELECT state,
	   COUNT(gender) AS 'Male',
	   COUNT(gender) AS 'Female'
FROM Lab_02
GROUP BY state
HAVING COUNT(*) >= 10
ORDER BY state

SELECT * 
FROM Users
WHERE UserName = 'mmack' AND Password = 'WiNnEr'
```



#### FROM a txt file

```mysql
BULK INSERT Employees 
FROM 'c:\temp\HW_01-2_NewEmployees.txt'
WITH (
	FIRSTROW = 2,
	ROWTERMINATOR = '\n',
	FIELDTERMINATOR = '\t',
	KEEPIDENTITY
)
```



#### DECLARE, TIES

```mysql
DECLARE @cnt INT = 5;
	SELECT TOP(@cnt) WITH TIES VendorID, InvoiceDate
	FROM dbo.Invoices
	ORDER BY InvoiceDate
```



#### CREATE, IF, ELSE    

```mysql
CREATE PROCEDURE spGetVendors
		@pageNum INT = 0,
		@records INT = -1
	AS
		SET NOCOUNT ON
		IF(@records = -1) BEGIN
			SELECT	VendorName, VendorCity, 
					VendorState, VendorZipCode
			FROM dbo.Vendors
			ORDER BY VendorState, VendorCity
		END ELSE BEGIN
			SELECT	VendorName, VendorCity, 
					VendorState, VendorZipCode
			FROM dbo.Vendors
			ORDER BY VendorState, VendorCity
				OFFSET (@pageNum * @records) ROWS
				FETCH NEXT @records ROWS ONLY
		END
```

​	

#### CONVERT AND CAST

数据格式转换。

convert包括了cast的所有功能，而且convert还能进行日期转换，cast是ANSI兼容的，而convert则不是。

```mysql
SELECT 'Invoice: #' 

 + InvoiceNumber
   + ', dated ' 
   + CONVERT(char(8), PaymentDate, 1)
   + ' for $' 
   + CONVERT(varchar(9), PaymentTotal, 1)
     FROM Invoices;

SELECT PaymentDate,
	CAST(PaymentDate AS DATE) AS CastDate,
	CAST(PaymentDate AS char(11)) AS CastChar
FROM Invoices;

SELECT CAST(PaymentTotal AS INT) / 10, 
		CAST(PaymentTotal AS INT) % 10,
		'$' + CAST(PaymentTotal AS varchar(20)),
		CONVERT(CHAR(8), PaymentDate, 1),
		CONVERT(CHAR(8), PaymentDate, 2),
		CONVERT(VARCHAR(100), PaymentTotal, 1),
		CAST(PaymentDate AS VARCHAR(19)),
		CAST(PaymentDate AS VARCHAR(11)),
		GETDATE(),
		CAST(GETDATE() AS VARCHAR(19))
FROM dbo.Invoices
```



#### Two table JOIN

```mysql
SELECT v.VendorName, 
	   i.InvoiceNumber, 
	   i.InvoiceDate,
	   (InvoiceTotal - PaymentTotal - CreditTotal) AS Balance
FROM Vendors			v
	JOIN	Invoices	i
		ON v.VendorID = i.InvoiceID
WHERE (InvoiceTotal - PaymentTotal - CreditTotal) > 0
ORDER BY VendorName
```


和上面一样但是不用JOIN

```mysql
SELECT VendorName, 
	   InvoiceNumber, 
	   InvoiceDate,
	   (InvoiceTotal - PaymentTotal - CreditTotal) AS Balance
FROM Vendors	v,
	 Invoices	i
WHERE v.VendorID = i.InvoiceID AND 
	 (InvoiceTotal - PaymentTotal - CreditTotal) > 0
ORDER BY VendorName
```



#### LEFT JOIN, RIGHT JOIN

SELECT v.VendorName, SUM(i.Balance) AS Balance
FROM Vendors				v

--4 different ways from(?) parent to child
	JOIN vwInvoices	i
	--LEFT JOIN vwInvoices	i
	--RIGHT JOIN vwInvoices	i
		ON v.VendorID = i.VendorID
GROUP BY v.VendorName、

#### FULL JOIN 

```mysql
SELECT d.DeptNo, d.DeptName
FROM dbo.Departments		d
	FULL JOIN Employees		e
		ON d.DeptNo = e.DeptNo
WHERE d.DeptNo IS NULL OR e.EmployeeID IS NULL
ORDER BY d.DeptName
```



#### CROSS JOIN

```mysql
SELECT d.DeptNo, d.DeptName, e.LastName
FROM dbo.Departments		d
	CROSS JOIN Employees	e
-- CROSS can use " , "
--	, Employees		e
ORDER BY d.DeptName
```



#### SELF JOIN

```mysql
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City 
ORDER BY A.City;
```



#### DISTINCT(不重复), CASE

```mysql
SELECT DISTINCT r.renterName,
		p.address,
		pr.deposit,
		[depositOwed] = rd.depositReq - pr.deposit,
		[status] = CASE
			WHEN rd.depositReq - pr.deposit = 0 THEN	'OK'
			WHEN rd.depositReq - pr.deposit > 0 THEN	'Outstanding Account'
			WHEN rd.depositReq - pr.deposit < 0 THEN    'Refund Needed'
			END 

FROM Renter r
		JOIN dbo.PropertyRental pr
			ON pr.renterId = r.renterId
		JOIN dbo.RentalData rd
			ON rd.rentalDataId = pr.rentalDataId
		JOIN dbo.Property p
			ON p.propertyId = rd.propertyId
ORDER BY r.renterName
```



#### top 5, top 5% and next 5

1). top 5

```mysql
SELECT TOP 5 r.renterName,
	p.address
FROM  Renter AS r 
	JOIN dbo.PropertyOwners  po
		ON po.propertyId = r.renterId
	JOIN dbo.Property p
		ON p.propertyId = r.renterId
WHERE (percentOwner = 100)
ORDER BY p.address, r.renterName
```

2). next 5, OFFEST	FETCH NEXT

```mysql
SELECT  r.renterName,
		p.address
FROM  Renter AS r 
		JOIN dbo.PropertyOwners  po
			ON po.propertyId = r.renterId
		JOIN dbo.Property p
			ON p.propertyId = r.renterId
WHERE percentOwner = 100
ORDER BY p.address, r.renterName 
	OFFSET 5 ROWS
		FETCH NEXT 5 ROWS ONLY
```

3). top 5%

```mysql
SELECT TOP(5) PERCENT VendorID, InvoiceTotal
FROM Invoices
ORDER BY InvoiceTotal DESC;
```



#### use, drop

```mysql
USE [AP]
DROP Table Users
```



#### logical operators: AND, OR, and NOT

```mysql
The AND operator
WHERE VendorState = 'NJ' AND YTDPurchases > 200
The OR operator
WHERE VendorState = 'NJ' OR YTDPurchases > 200
The NOT operator
WHERE	NOT (InvoiceTotal >= 5000) 
		OR 
  		NOT (InvoiceDate <= '2016-07-01')
The same condition without the NOT operator
WHERE (InvoiceTotal < 5000) AND (InvoiceDate <= '2016-07-01')
```



#### UNION

The UNION operator selects only distinct values by default. To allow duplicate values, use UNION ALL.
The column names in the result-set are usually equal to the column names in the first SELECT statement in the UNION.

```mysql
WITH tbl AS(
		SELECT [status] = 'Paid in full', VendorID, Balance
		FROM dbo.vwInvoices
		WHERE Balance = 0
	UNION
		SELECT [status] = 'Outstanding balance', VendorID, Balance
		FROM dbo.vwInvoices
		WHERE Balance > 0
)	SELECT v.VendorName, tbl.status, tbl.VendorID, tbl.Balance
	FROM tbl
		JOIN dbo.Vendors v
			ON v.VendorID = tbl.VendorID
	WHERE tbl.Balance > 200
```

​	

#### DELETE, TRUNCATE

```mysql
DELETE FROM dbo.Exams WHERE examId = 1

DELETE FROM Employees
TRUNCATE TABLE Employees
```



#### IN, LIKE

in后条件不多，可以考虑主表建索引，或用union all 代替。

```mysql
SELECT *
FROM tb
WHERE id IN (1, 2, 3, 4, ........)
AND NAME = 'best'
```

```mysql
SELECT * 
FROM Customers
WHERE Country IN (SELECT Country FROM Suppliers);
```

```mysql
SELECT * 
FROM Customers
WHERE Country NOT IN ('Germany', 'France', 'UK');
```

`WHERE CustomerName LIKE 'a%'`	Finds any values that start with "a"
`WHERE CustomerName LIKE '%a'`	Finds any values that end with "a"
`WHERE CustomerName LIKE '%or%'`	Finds any values that have "or" in any position
`WHERE CustomerName LIKE '_r%'`	Finds any values that have "r" in the second position
`WHERE CustomerName LIKE 'a_%_%'`	Finds any values that start with "a" and are at least 3 characters in length
`WHERE ContactName LIKE 'a%o'`	Finds any values that start with "a" and ends with "o"

#### Primary and Foreign keys

```mysql
DROP TABLE Scores
DROP TABLE Exams

CREATE TABLE Exams(
	examId		INT			PRIMARY KEY		IDENTITY,
	examName	VARCHAR(20),
	examYear	INT
)

CREATE TABLE Scores(
	scoreId		INT		PRIMARY KEY		IDENTITY,
	--examId		INT,
	examId		INT		FOREIGN KEY REFERENCES dbo.Exams(examId),

	average		FLOAT

)

INSERT	INTO	dbo.Exams
VALUES('test1', 2000),('test1', 2001),('test2', 2000)

INSERT INTO dbo.Scores
VALUES(1, 89),(2,79),(3,99)

--DELETE FROM dbo.Exams WHERE examId = 1

SELECT e.examname, e.examYear, s.average
FROM dbo.Exams e
	RIGHT JOIN dbo.Scores s
		ON s.examId = e.examId
```



#### alias

```mysql
SELECT column_name AS alias_name
FROM table_name;
```



#### TRY_CONVERT(), CONVERT(), CAST()

```mysql
syntax: CONVERT(value, type)
		CAST(value as type)

SELECT CAST("2017-08-29" AS DATE);
SELECT CAST(150 AS CHAR);
SELECT CAST("14:06:10" AS TIME);

SELECT CONVERT("2017-08-29", DATE);
SELECT CONVERT(150, CHAR);
SELECT CONVERT("14:06:10", TIME);

SELECT TOP(1)
	InvoiceDate,
	--CONVERT(VARCHAR, InvoiceDate),
	--CONVERT(VARCHAR, InvoiceDate, 1),
	--CONVERT(VARCHAR, InvoiceDate, 2),
	--CONVERT(VARCHAR, InvoiceDate, 102),
	--CONVERT(VARCHAR, InvoiceDate, 3),
	--CONVERT(VARCHAR, InvoiceDate, 7),
	--CONVERT(VARCHAR, InvoiceDate, 107),
	--TRY_CONVERT(INT, 'Hello')
	CASE WHEN TRY_CONVERT(FLOAT, 'Hello') IS NULL
		THEN 'Did not cast'
		ELSE 'Cast worked'
	END
FROM dbo.Invoices
```



#### CASE WHEN

根据 case when 新的 sort字段排序

```mysql
SELECT OrderID, Quantity,
CASE
    WHEN Quantity > 30 THEN "The quantity is greater than 30"
    WHEN Quantity = 30 THEN "The quantity is 30"
    ELSE "The quantity is under 30"
END
FROM OrderDetails;
```



当colume 与condition 条件相等时结果为result

```mysql
case colume 
 when condition then result
 when condition then result
 when condition then result
else result
end
```



当满足某一条件时，执行某一result

```mysql
case  
 when condition then result
 when condition then result
 when condition then result
else result
end
```



当满足某一条件时，执行某一result,把该结果赋值到new_column_name 字段中

```mysql
case
 when condition then result
 when condition then result
 when condition then result
else result
end new_column_name
```



#### IIF()

```mysql
syntax: IIF ( boolean_expression, true_value, false_value )

DECLARE @a int = 45, @b int = 40;  
SELECT IIF ( @a > @b, 'TRUE', 'FALSE' ) AS Result

IIF(i.Balance = 0, 'OK', 'Outstanding') AS Status
```



#### TRIGGER

创建有多个执行语句的触发器
syntax: 

```mysql
CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
ON 表名 (FOR EACH ROW)
BEGIN
    执行语句列表
END
```

example:

```mysql
CREATE TRIGGER termsLogTrigger ON dbo.Terms
AFTER INSERT, UPDATE ← 六种触发器：BEFORE INSERT, BEFORE DELETE, BEFORE UPDATE, 
								   AFTER INSERT, AFTER DELETE, AFTER UPDATE
AS 
BEGIN
	INSERT INTO TermsLogHistory
	SELECT i.TermID
		   ,i.TermsDescription
		   ,i.TermDueDays
		   ,i.isDeleted
		   ,i.SUSER_NAME()
		   ,i.GETDATE()
	FROM Inserted i
		JOIN dbo.Terms t ON t.TermsID = i.TermsID
END
```



#### TRY...CATCH

```mysql
TRY BEGIN CATCH
		IF(@@TRANCOUNT > 0) ROLLBACK TRAN
		DECLARE @params VARCHAR(MAX);
		SELECT @params =  '@TermsId=' + CAST(@TermsId AS VARCHAR(10)) +
						  '@TermsDescription=' + @TermsDescription +
						  '@TermsDueDays=' + CAST(@TermsDueDays AS VARCHAR(10)) +
						  '@isDeleted=' + CAST(@isDeleted AS VARCHAR(2))
		EXEC spRecordError @params
	END CATCH
```



#### SUSER_NAME() or just SYSTEM_USER

```mysql
CREATE TRIGGER termsLogTrigger ON dbo.Terms
AFTER INSERT, UPDATE
AS
BEGIN
	INSERT INTO TermsLogHistory
	SELECT 	 i.TermsID			
			,i.TermsDescription
			,i.TermsDueDays	
			,i.isDeleted		
			,SUSER_NAME()
			,GETDATE()		
	FROM Inserted i 
		JOIN Terms t ON t.TermsID = i.TermsID
END
GO
```



#### HOST_NAME()

```mysql
CREATE TABLE Orders
   (OrderID     int        PRIMARY KEY,
    CustomerID  nchar(5)   REFERENCES Customers(CustomerID),
    Workstation nchar(30)  NOT NULL DEFAULT HOST_NAME(),
    OrderDate   datetime   NOT NULL,
    ShipDate    datetime   NULL,
    ShipperID   int        NULL REFERENCES Shippers(ShipperID));
GO  
```



#### NEWID()

```mysql
FROM dbo.Vendors	v
	JOIN dbo.GLAccounts	gla 
		ON v.DefaultAccountNo = gla.AccountNo
	JOIN dbo.Terms t ON v.DefaultTermsID = t.TermsID
WHERE v.VendorID IN (SELECT VendorId FROM dbo.Invoices)
ORDER BY NEWID(), v.VendorName
FOR JSON PATH, ROOT('Vendors')
```



#### @@IDENTITY

MYSQL获取自增ID
select @@IDENTITY;

@@identity 是表示的是最近一次向具有identity属性(即自增列)的表插入数据时对应的自增列的值，是系统定义的全局变量。
一般系统定义的全局变量都是以@@开头，用户自定义变量以@开头。



#### ERROR_NUMBER(), ERROR_SEVERITY(), ERROR_STATE(), ERROR_PROCEDURE(), ERROR_LINE(), ERROR_MESSAGE()

```mysql
CREATE TABLE errorLog (
	errorLogId		INT		PRIMARY KEY		IDENTITY,
	error_number	INT,
	error_severity	INT,
	error_state		INT,
	error_procedure	VARCHAR(20),
	error_line		INT,
	error_message	VARCHAR(20),
	param_list		VARCHAR(MAX),
	userName		VARCHAR(20),
	error_date		DATETIME

)
GO
```

```mysql
CREATE PROCEDURE spRecordError
	@param_list VARCHAR(MAX) = ''
AS
BEGIN
	INSERT INTO dbo.errorLog VALUES (
		ERROR_NUMBER(), ERROR_SEVERITY(), ERROR_STATE(), ERROR_PROCEDURE(),
		ERROR_LINE(), ERROR_MESSAGE(), @param_list, SUSER_NAME(), GETDATE()
	)
END
GO
```



#### BEGIN TRAN, ROLLBACK TRAN, COMMIT TRAN

```mysql
BEGIN TRAN
	SELECT SUM(balance), MAX(CreditTotal)  FROM dbo.vwInvoices
	WHILE (SELECT SUM(balance) FROM dbo.vwInvoices) >= 2000 BEGIN
		UPDATE dbo.vwInvoices 
		SET CreditTotal = CreditTotal + .05
		WHERE Balance > 0

		IF(SELECT MAX(CreditTotal) FROM dbo.vwInvoices) >= 3000
			BREAK
		ELSE
			CONTINUE        
		PRINT 'Hello...'
	END
	
	SELECT SUM(balance), MAX(CreditTotal)  FROM dbo.vwInvoices

ROLLBACK TRAN
```



#### CURSOR

```mysql
DECLARE @tblName VARCHAR(255)

DECLARE tblCursor CURSOR FOR (
	SELECT TABLE_NAME
	FROM INFORMATION_SCHEMA.TABLES
	WHERE TABLE_TYPE = 'BASE TABLE' 
)

OPEN tblCursor

FETCH NEXT FROM tblCursor INTO @tblName

WHILE @@FETCH_STATUS = 0 BEGIN
	PRINT 'Indexing table: ' + @tblName

	EXEC ('ALTER INDEX ALL ON ' + @tblName + ' REORGANIZE')
	
	FETCH NEXT FROM tblCursor INTO @tblName

END

CLOSE tblCursor
DEALLOCATE tblCursor
```



```mysql
XML / JSON output from query
									   & TRUNCATE TABLE DELETE FROM table
									   & XML parameter into procedure
```



```mysql
SELECT	TOP(5)
		v.VendorID, 
		v.VendorName,
		v.VendorCity,
		v.VendorState,
		gla.AccountDescription,
		t.TermsDescription,
		[Invoices] = (
			SELECT	i.InvoiceID,
					i.InvoiceNumber,
					i.InvoiceTotal,
					it.TermsDescription,
					i.Balance,
					IIF(i.Balance = 0, 'OK', 'Outstanding') AS Status,
					[Items] = (
						SELECT	ili.InvoiceLineItemAmount, 
								ili.InvoiceLineItemDescription,
								igla.AccountDescription
						FROM dbo.InvoiceLineItems ili
							JOIN dbo.GLAccounts igla 
								ON ili.AccountNo = igla.AccountNo
						WHERE ili.InvoiceID = i.InvoiceID
						FOR JSON PATH
					)
			FROM dbo.vwInvoices i
				JOIN dbo.Terms it ON i.TermsID = it.TermsID
			WHERE i.VendorID = v.VendorID
			FOR JSON PATH
		)

FROM dbo.Vendors	v
	JOIN dbo.GLAccounts	gla 
		ON v.DefaultAccountNo = gla.AccountNo
	JOIN dbo.Terms t ON v.DefaultTermsID = t.TermsID
WHERE v.VendorID IN (SELECT VendorId FROM dbo.Invoices)
ORDER BY NEWID(), v.VendorName
FOR JSON PATH, ROOT('Vendors')


TRUNCATE TABLE dbo.Guests
INSERT INTO dbo.Guests VALUES (4, 'Tommy')
INSERT INTO dbo.Guests VALUES (4, 'Jack')

DECLARE @xml AS XML = '<Guests>
							<add inviteeId = "2" name = "Mike" />
							<add inviteeId = "2" name = "Don" />
							<add inviteeId = "2" name = "Lucille" />
							<delete guestId = "2" />
							<update guestId = "1" name = "Tom" />
						</Guests>'
```

​					   

#### INSERT

```mysql
SELECT * FROM dbo.Guests

INSERT INTO Guests (inviteeId, guestName)
SELECT	ent.value('@inviteeId', 'int'), 
		ent.value('@name', 'varchar(50)')
FROM @xml.nodes('/Guests/add') foo(ent)
```



#### DELETE

```mysql
DELETE FROM Guests WHERE guestId IN(
	SELECT ent.value('guestId', 'int')
	FROM @xml.nodes('/Guests/delete') foo(ent)
)
```



#### UPDATE

```mysql
UPDATE dbo.Guests
SET guestName = tbl.name
FROM (
	SELECT	[gid] = ent.value('@guestId', 'int'), 
			[name] = ent.value('@name', 'varchar(50)')
	FROM @xml.nodes('/Guests/update') foo(ent)
	)
	AS tbl
	WHERE guestId = tbl.gid

	SELECT * FROM dbo.Guests
```



#### BULK INSERT

```mysql
FROM a txt file

BULK INSERT Employees 
FROM 'c:\temp\HW_01-2_NewEmployees.txt'
WITH (
FIRSTROW = 2,
ROWTERMINATOR = '\n',
FIELDTERMINATOR = '\t',
KEEPIDENTITY
)
```



#### JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN, CROSS JOIN

exam 1 review 8-13



#### Indexing

```mysql
DECLARE @tblName VARCHAR(255)

DECLARE tblCursor CURSOR FOR (
	SELECT TABLE_NAME
	FROM INFORMATION_SCHEMA.TABLES
	WHERE TABLE_TYPE = 'BASE TABLE' 
)

OPEN tblCursor

FETCH NEXT FROM tblCursor INTO @tblName

WHILE @@FETCH_STATUS = 0 BEGIN
	PRINT 'Indexing table: ' + @tblName

	EXEC ('ALTER INDEX ALL ON ' + @tblName + ' REORGANIZE')

	FETCH NEXT FROM tblCursor INTO @tblName
END

CLOSE tblCursor
DEALLOCATE tblCursor
```



#### DROP TABLE IF EXISTS tableName

#### WHILE() & BEGIN ... END & IF ... ELSE


```mysql
BEGIN TRAN
	SELECT SUM(balance), MAX(CreditTotal)  FROM dbo.vwInvoices
	WHILE (SELECT SUM(balance) FROM dbo.vwInvoices) >= 2000 BEGIN
		UPDATE dbo.vwInvoices 
		SET CreditTotal = CreditTotal + .05
		WHERE Balance > 0

		IF(SELECT MAX(CreditTotal) FROM dbo.vwInvoices) >= 3000
			BREAK
		ELSE
			CONTINUE        
		PRINT 'Hello...'
	END

	SELECT SUM(balance), MAX(CreditTotal)  FROM dbo.vwInvoices
ROLLBACK TRAN


```



### 补充

#### <> is !=
