# sql-project-
Creating Tables, Data base and Generating ERD(1NF,2NF,3NF)


/* ISC 561 FINAL PROJECT
    VENKATA S YARLAGADDA 
	J00642870 */
	use master
	 drop database PrancingPaws
	 GO

 CREATE DATABASE PrancingPaws
 GO



 USE PrancingPaws
 GO

 /* Create our tables */

 SET NOCOUNT ON

 CREATE TABLE tbCustomer
 
 (	
	CustomerID INT IDENTITY(1,2) PRIMARY KEY,
	FirstName NVARCHAR(50) NOT NULL,
	LastName NVARCHAR(50) NOT NULL,
	Address NVARCHAR(50) NOT NULL,
	City NVARCHAR(25) NOT NULL,
	State VARCHAR(20) NOT NULL,
	Zipcode CHAR(6) NOT NULL,
	Phonenumber CHAR(10) NOT NULL
	

)
GO


CREATE TABLE tbWalker
(
    EmployeeID INT IDENTITY(2,2) PRIMARY KEY,
	FirstName NVARCHAR(50) NOT NULL,
	LastName NVARCHAR(50) NOT NULL,
	Phonenumber CHAR(10) NOT NULL
)
GO

CREATE TABLE tbCompany
(
    CompanyID INT IDENTITY(2,2) PRIMARY KEY,
	Name NVARCHAR(50) NOT NULL,
	Address NVARCHAR(50) NOT NULL,
	City NVARCHAR(25) NOT NULL,
	State VARCHAR(20) NOT NULL,
	Zipcode CHAR(6) NOT NULL,
	Phonenumber CHAR(10) NOT NULL,
	Email CHAR(100) NOT NULL,
)
GO

CREATE TABLE tbPet
(
    PetID INT IDENTITY(2,2) PRIMARY KEY,
	Name NVARCHAR(50) NOT NULL,
	Species NVARCHAR(50) NOT NULL
)
GO

CREATE TABLE tbInvoice
(
    InvoiceNo INT IDENTITY(1,2) PRIMARY KEY,
	transcationDate DATE NOT NULL DEFAULT GETDATE(),
	TotalDue MONEY NOT NULL DEFAULT 0.0,
	Remarks VARCHAR(100),
	Message VARCHAR(100)

)
GO

CREATE TABLE tbList
(
	ListID INT IDENTITY (100,3) PRIMARY KEY,
	Quantity INT NOT NULL,
	Total MONEY NOT NULL
)
GO


CREATE TABLE tbProduct
(
	ProductID INT IDENTITY (100,3) PRIMARY KEY,
	UnitPrice MONEY NOT NULL DEFAULT 0.0,
	Description VARCHAR(500)
)
GO


PRINT 'Sucessfully created tables in database'
SET NOCOUNT OFF
GO


/* Check range Constraint adding */

ALTER TABLE tbList
ADD CONSTRAINT list_quantity_check 
CHECK (Quantity > 0)
GO

PRINT ' successfully CHECK Range Constraint added '
GO

/* Unique Constraints*/

ALTER TABLE tbProduct
ADD CONSTRAINT product_unitprice_unique
UNIQUE (UnitPrice)                          --Candidate key for table tblProduct
GO

PRINT 'UNIQUE Constraints successfully applied'
GO


/* Enforcing Referential integrity constraints to our tables */

/* For table tbInvoice */

ALTER TABLE tbInvoice
ADD CustomerID INT NOT NULL
GO

ALTER TABLE tbInvoice
ADD CONSTRAINT customer_invoice_fk FOREIGN KEY (CustomerID)
REFERENCES tbCustomer (CustomerID)
GO


ALTER TABLE tbInvoice
ADD EmployeeID INT NOT NULL
GO

ALTER TABLE tbInvoice
ADD CONSTRAINT walker_invoice_fk FOREIGN KEY (EmployeeID)
REFERENCES tbWalker (EmployeeID)
GO

ALTER TABLE tbInvoice
ADD CompanyID INT NOT NULL
GO

ALTER TABLE tbInvoice
ADD CONSTRAINT company_invoice_fk FOREIGN KEY (CompanyID)
REFERENCES tbCompany (CompanyID)
GO

ALTER TABLE tbInvoice
ADD PetID INT NOT NULL
GO

ALTER TABLE tbInvoice
ADD CONSTRAINT pet_invoice_fk FOREIGN KEY (PetID)
REFERENCES tbPet (PetID)
GO


/* For table tbList */

ALTER TABLE tbList
ADD InvoiceNo INT NOT NULL
GO

ALTER TABLE tbList
ADD CONSTRAINT invoice_list_fk FOREIGN KEY (InvoiceNo)
REFERENCES tbInvoice (InvoiceNo)
GO

ALTER TABLE tbList
ADD ProductID INT NOT NULL
GO

ALTER TABLE tbList
ADD CONSTRAINT product_list_fk FOREIGN KEY (ProductID)
REFERENCES tbProduct (ProductID)
GO

PRINT 'Referential integrity Constraints successfully enforced'
GO


/*Inserting data into tables*/

/*INSERTING CUSTOMERS into tbCustomer*/

SET NOCOUNT ON

BEGIN TRY 

BEGIN TRANSACTION 

INSERT INTO tbCustomer (FirstName,LastName,Address,City,State,Zipcode,Phonenumber)
  VALUES ('Power','Star','133 East drive','Mobile','Alabama','36608','2589631478')

INSERT INTO tbCustomer (FirstName,LastName,Address,City,State,Zipcode,Phonenumber)
  VALUES ('venkat','yarlagadda','143 south drive','Montgomery','Alabama','63308','2541369852')

INSERT INTO tbCustomer (FirstName,LastName,Address,City,State,Zipcode,Phonenumber)
  VALUES ('shreya','goshal','158 west drive','Denver','Colorodo','523001','2536984125')


COMMIT TRANSACTION

PRINT 'Customers are successfully Inserted'
END TRY

BEGIN CATCH 
   DECLARE @ErrorMessage VARCHAR(100)
   SET @ErrorMessage = ERROR_MESSAGE() + 'Rolledback transaction: Error at Customers insertions'
   ROLLBACK TRANSACTION 
   RAISERROR (@ErrorMessage, 16,1)
END CATCH
GO

select * from tbCustomer



/*INSERTING CUSTOMERS into tbWalker*/

SET NOCOUNT ON

BEGIN TRY 

BEGIN TRANSACTION 

INSERT INTO tbWalker(FirstName,LastName,Phonenumber)
  VALUES ('mega','Star','9356912589')


INSERT INTO tbWalker(FirstName,LastName,Phonenumber)
  VALUES ('kiran','kanampallli','7204195896')

INSERT INTO tbWalker(FirstName,LastName,Phonenumber)
  VALUES ('suresh','raina','1452689325')


COMMIT TRANSACTION

PRINT 'Walkers are successfully Inserted'
END TRY

BEGIN CATCH 
   DECLARE @ErrorMessage VARCHAR(100)
   SET @ErrorMessage = ERROR_MESSAGE() + 'Rolledback transaction: Error at Customers insertions'
   ROLLBACK TRANSACTION 
   RAISERROR (@ErrorMessage, 16,1)
END CATCH
GO

select * from tbWalker



/*INSERTING  into tbCompany*/

SET NOCOUNT ON

BEGIN TRY 

BEGIN TRANSACTION 

INSERT INTO tbCompany(Name,Address,City,State,Zipcode,Phonenumber,Email)
  VALUES ('Google','1675 Airport blvd','Illions','chicago','60007','2561476278','google.@gmail.com')

INSERT INTO tbCompany (Name,Address,City,State,Zipcode,Phonenumber,Email)
  VALUES ('Amazon','1587 Government blvd','Hostville','Newyork','10001','1258963478','amazon.@gmail.com')

INSERT INTO tbCompany (Name,Address,City,State,Zipcode,Phonenumber,Email)
  VALUES ('Musigma','523 suriseri','Chennai','Tamilnadu','80014','9345698523','Musigma.@gmail.com')


COMMIT TRANSACTION

PRINT 'Company are successfully Inserted'
END TRY

BEGIN CATCH 
   DECLARE @ErrorMessage VARCHAR(100)
   SET @ErrorMessage = ERROR_MESSAGE() + 'Rolledback transaction: Error at Customers insertions'
   ROLLBACK TRANSACTION 
   RAISERROR (@ErrorMessage, 16,1)
END CATCH
GO


select * from tbCompany


/*INSERTING  into tbPet*/

SET NOCOUNT ON

BEGIN TRY 

BEGIN TRANSACTION 

INSERT INTO tbPet(Name,Species)
  VALUES ('Jimmy','Wolf')

INSERT INTO tbPet(Name,Species)
  VALUES ('tommy','Bush dog')

  INSERT INTO tbPet(Name,Species)
  VALUES ('sweety','African wild dog')

COMMIT TRANSACTION

PRINT 'Pet names are successfully Inserted'
END TRY

BEGIN CATCH 
   DECLARE @ErrorMessage VARCHAR(100)
   SET @ErrorMessage = ERROR_MESSAGE() + 'Rolledback transaction: Error at Customers insertions'
   ROLLBACK TRANSACTION 
   RAISERROR (@ErrorMessage, 16,1)
END CATCH
GO

select * from tbPet

/* INSERTING INVOICES into tbInvoice*/

BEGIN TRY 

BEGIN TRANSACTION



DECLARE @CustomerID INT
SET @CustomerID = (SELECT CustomerID
                   FROM tbCustomer
				   WHERE FirstName = 'Power' AND LastName = 'Star')

DECLARE @EmployeeID INT
SET @EmployeeID = (SELECT EmployeeID
                   FROM tbWalker
				    WHERE PhoneNumber = '9356912589')

DECLARE @CompanyID INT
SET @CompanyID = (SELECT CompanyID
                  FROM tbCompany
				   WHERE Email = 'google.@gmail.com')

DECLARE @PetID INT
SET @PetID = (SELECT PetID
                        FROM tbPet
						WHERE Species = 'Wolf')

INSERT INTO tbInvoice (transcationDate, TotalDue, Remarks, Message, CustomerID, EmployeeID, CompanyID, PetID)
	VALUES ('12/1/2018', '63.00', 'Extra charge has applied for Extra medicine', 'meet office frquently for two weeks', @CustomerID, @EmployeeID, @CompanyID,@PetID)

INSERT INTO tbInvoice (transcationDate, TotalDue, Remarks, Message,CustomerID,EmployeeID,CompanyID,PetID)
	VALUES ('12/5/2019', '69.60', 'Extra charge has applied  tax and medicines returned ', 'thank you visit again', @CustomerID, @EmployeeID, @CompanyID,@PetID)

INSERT INTO tbInvoice (transcationDate, TotalDue, Remarks, Message,CustomerID,EmployeeID,CompanyID,PetID)
	VALUES ('10/6/2018', '125.3', 'Extra charge has applied for medicine and special treatment ', 'Thankyou', @CustomerID, @EmployeeID, @CompanyID,@PetID)

COMMIT TRANSACTION

PRINT 'Invoices are succesfully Inserted.'
END TRY

BEGIN CATCH 
   DECLARE @ErrorMessage VARCHAR(100)
   SET @ErrorMessage = ERROR_MESSAGE() + 'Rolledback transaction: Error at Invoices insertions'
   ROLLBACK TRANSACTION 
   RAISERROR (@ErrorMessage, 16,1)
END CATCH
GO


/* INSERTING Products into tbProduct */


BEGIN TRY

BEGIN TRANSACTION 

INSERT INTO tbProduct(UnitPrice,Description)
VALUES ('2.45','Use three times a day')

INSERT INTO tbProduct(UnitPrice,Description)
VALUES ('5.36','Use two times a day')

INSERT INTO tbProduct(UnitPrice,Description)
VALUES ('58.3','Use one times a day')

COMMIT TRANSACTION 

PRINT 'Lists are successfully inserted'
END TRY

BEGIN CATCH 
   DECLARE @ErrorMessage VARCHAR(100)
   SET @ErrorMessage = ERROR_MESSAGE() + 'Rolledback transaction: Error at Products insertions'
   ROLLBACK TRANSACTION 
   RAISERROR (@ErrorMessage, 16,1)
END CATCH
GO


select * from tbProduct


/* INSERTING InvoiceProducts into tbList */

BEGIN TRY

BEGIN TRANSACTION

DECLARE @InvoiceNo INT
SET @InvoiceNo = (SELECT InvoiceNo 
                  FROM tbInvoice 
				  WHERE TotalDue='63.00')

DECLARE @ProductID INT
SET @ProductID = (SELECT ProductID 
                  FROM tbProduct 
				  WHERE UnitPrice ='2.45')

INSERT INTO tbList(Quantity, Total,InvoiceNo,ProductID)
	VALUES ('4','288', @InvoiceNo, @ProductID)

INSERT INTO tbList(Quantity, Total,InvoiceNo,ProductID)
	VALUES ('2', '269.98', @InvoiceNo, @ProductID)

COMMIT TRANSACTION
PRINT 'Inserted InvoiceProducts Successfully'
END TRY
BEGIN CATCH 
   DECLARE @ErrorMessage VARCHAR(100)
   SET @ErrorMessage = ERROR_MESSAGE() + 'Rolledback transaction: Error at InvoiceProducts insertions'
   ROLLBACK TRANSACTION 
   RAISERROR (@ErrorMessage, 16,1)
END CATCH
GO

select * from tbList

/* Stored procedure for INSERTing a new PRODUCT into tbProduct  */
GO
CREATE PROC NewProduct

@UnitPrice MONEY,
@Description VARCHAR(50) --Input Parameters
AS
BEGIN
  BEGIN TRY 

    BEGIN TRANSACTION

    --Transaction
    INSERT INTO tbProduct(UnitPrice, Description) VALUES (@UnitPrice,@Description)

    COMMIT TRANSACTION

  END TRY
  BEGIN CATCH
    DECLARE @ErrorMessage VARCHAR(500)
    SET @ErrorMessage = ERROR_MESSAGE() + ' Rolledback transaction: New Product Insert.'
    ROLLBACK TRANSACTION
    RAISERROR (@ErrorMessage, 16,1)
 END CATCH
END

GO


--Test cases for regression testing
/*
 DECLARE  @UnitPrice MONEY,
         @Description VARCHAR(50)
SET @UnitPrice = '143'
SET @Description = 'it Works'
EXEC NewProduct  @UnitPrice,@Description
GO 

select * from tbProduct

*/
/* Stored procedure for UPDATEING a PRODUCT in tbProduct  */


CREATE PROC UpdateProduct
@ProductID INT,
@UnitPrice MONEY,
@Description VARCHAR(50)

AS
BEGIN
   BEGIN TRY
     BEGIN TRANSACTION

       --Transaction
       UPDATE tbProduct SET  UnitPrice = @UnitPrice, Description = @Description
       WHERE tbProduct.ProductID = @ProductID

       COMMIT TRANSACTION

   END TRY
   BEGIN CATCH
     DECLARE @ErrorMessage VARCHAR(500)
     SET @ErrorMessage = ERROR_MESSAGE() + ' Rolledback transaction: Product Update.'
     ROLLBACK TRANSACTION
     RAISERROR (@ErrorMessage, 16,1)
   END CATCH

END

GO



--Test cases for regression testing
/*
DECLARE @ProductID INT,
	    @UnitPrice MONEY ,
		@Description VARCHAR(50)
SET @ProductID = 8
SET @UnitPrice = '143.00'
SET @Description = 'it works'
EXEC UpdateProduct @ProductID, @UnitPrice,@Description
GO   

*/


/* Stored procedure for DELETEING a PRODUCT in tbProduct  */


CREATE PROC DeleteProduct
@ProductID INT
AS
BEGIN
   BEGIN TRY
     BEGIN TRANSACTION

        --Transaction
        DELETE tbProduct
        WHERE tbProduct.ProductID = @ProductID

     COMMIT TRANSACTION

   END TRY
   BEGIN CATCH
     DECLARE @ErrorMessage VARCHAR(500)
     SET @ErrorMessage = ERROR_MESSAGE() + ' Rolledback transaction: Product Delete.'
     ROLLBACK TRANSACTION
     RAISERROR (@ErrorMessage, 16,1)
   END CATCH
END

GO


--Test cases for regression testing
 /* 
 DECLARE @ProductID INT
SET @ProductID = 109

EXEC DeleteProduct @ProductID
GO

SELECT * 
FROM tbProduct  

*/ 
/* Enforcing minimum cardinality on tbInvoice using a TRIGGER */

CREATE TRIGGER mincardinalityinvoicetrig ON tbList
FOR DELETE
AS
BEGIN

		IF(SELECT COUNT(*)
		   FROM tbList
		   WHERE tbList.Quantity = ( SELECT DISTINCT Quantity
		                       FROM deleted) ) < 1
		BEGIN
		ROLLBACK TRAN
		RAISERROR ( 'A child should must have atleast one parent.....!', 16, 1)
		END
END
GO

PRINT 'Successfully created the trigger to check for minimum cardinality between tbInvoice and tbList'
GO


/* Test cases
select * 
from tblist

DELETE FROM tblist
WHERE Quantity = 2
*/



/*Creating audit table for table tbProduct*/

CREATE TABLE tbProductAudit
(
	AuditLogID INT IDENTITY(1,1) PRIMARY KEY,
	ProductID INT,
	UnitPrice MONEY NOT NULL DEFAULT 0.0,
	Decsription VARCHAR(50),
	ChangedBy INT,
	ChangedTime DATETIME DEFAULT GETDATE(),
	ChangeType NVARCHAR(10),
	ImageType NVARCHAR(10)
)
GO

PRINT 'Audit table created successfully for Product table'
GO

ALTER TABLE tbProduct
ADD lastchangedby INT
GO

INSERT INTO tbProduct(UnitPrice,Description, lastchangedby)
VALUES ('345.23','Audit table is created sucessfully', 7)
GO

select * from tbProduct


--Creating an insertion trigger for audit table
GO
CREATE TRIGGER InsertProductforAudit ON tbProduct FOR INSERT
AS 
BEGIN
	INSERT INTO tbProductAudit( ProductID,
								 UnitPrice,
								 Decsription,
								 ChangedBy, 
								 ChangedTime,
								 ChangeType, 
								 ImageType)
	SELECT ProductID,
		   UnitPrice,
		   Description,
		   lastchangedby, 
		   GETDATE() AS ChangeTime, 
		   'INSERT' AS ChangeType, 
		   'AFTER' AS ImageType
	FROM inserted
END
GO
PRINT 'An Insertion trigger for audit table is created successfully...'
GO

/* Test Case

INSERT INTO tbProduct(UnitPrice,Description, lastchangedby)
VALUES ('1485.2', 'Thankyou for purchasing' , '9')
GO

*/

select * from tbproduct


--Cretaing an updation trigger for audit table
GO
CREATE TRIGGER UpdateProductforAudit ON tbProduct FOR UPDATE
AS 
BEGIN
	INSERT INTO tbProductAudit(ProductID,
								Unitprice,
								Decsription,
								ChangedBy, 
								ChangedTime, 
								ChangeType, 
								ImageType)
	SELECT ProductID, 
		   UnitPrice,
		   Description,
		   lastchangedby,
		   GETDATE() AS ChangeTime,
		   'UPDATED' AS ChangeType,
		   'BEFORE' AS ImageType
	FROM deleted

	
	INSERT INTO tbProductAudit(ProductID,
								UnitPrice,
								Decsription,
								ChangedBy,
								ChangedTime,
								ChangeType,
								ImageType)
	SELECT ProductID,
		   UnitPrice,
		   Description,
		   lastchangedby,
		   GETDATE() AS ChangeTime,
		   'INSERT' AS ChangeType,
		   'AFTER' AS ImageType
	FROM inserted
END
GO

PRINT 'An Updation trigger for audit table is created successfully...'	
GO


select * from tbProductAudit


/*
Test cases
UPDATE tbProduct
SET lastchangedby=10
WHERE ProductID = 121
GO
*/

select * from tbProductAudit


--Creating a deletion trigger for audit table
GO
CREATE TRIGGER DeleteProductforAudit ON tbProduct FOR DELETE
AS 
BEGIN
	INSERT INTO tbProductAudit(ProductID,
								UnitPrice,
								Decsription,
								ChangedBy,
								ChangedTime,
								ChangeType,
								ImageType)
	SELECT ProductID,
		   UnitPrice,
		   Description,
		   lastchangedby,
		   GETDATE() AS ChangeTime,
		   'DELETE' AS ChangeType,
		   'BEFORE' AS ImageType
	FROM deleted
END
GO
PRINT 'A deletion trigger for audit table is created successfully...'
GO


/*
Test Cases

DELETE FROM tbProduct
WHERE ProductID = 121
GO

*/
select * from tbProduct


/* Creating Views */

-- Creating a View that  hides a query containing a GROUPBY HAVING Statement

select * from tbProduct

GO

CREATE VIEW vwCostlyProducts
AS
SELECT COUNT(*) AS CostlyProducts, 
                   ProductID
FROM tbProduct
WHERE UnitPrice > '250.00'
GROUP BY ProductID
HAVING ProductID > 1
GO


PRINT 'A view that hides a query containing a GROUPBY HAVING Statement is created successfully'
GO

select * from vwCostlyProducts

-- Creating a View that  hides a query containing a sub query


GO

CREATE VIEW vwCostlyProductsSold
AS
SELECT * 
FROM tbList
WHERE ProductID IN (  SELECT ProductID
					  FROM	tbProduct
				      WHERE	tbProduct.UnitPrice > '2.00')

GO

PRINT 'A view that hides a query containing a nested subquery is created successfully'


select * from vwCostlyProductsSold
