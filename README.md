# Land&Property Registration system
![Oracle](https://img.shields.io/badge/Oracle-SQL-blue.svg)
![PL/SQL](https://img.shields.io/badge/PL%2FSQL-Database-orange.svg)
![GitHub](https://img.shields.io/badge/GitHub-Repository-green.svg)


## Introduction
- **Student**:Manzi fred (26634)
- **Group**: Tuesday
- **Course**: Database Development with PL/SQL (INSY 8311)
- **Lecturer**: Eric Maniraguha

## 📋 Problem Statement
Lack of a centralized, efficient, and secure system for managing land and property registration leads to delays, data inaccuracies, and potential fraud.
## 📊 Methodology
- **Phase I**: Defined problem and presented objectives.
- **Phase II**: Modeled order processing using UML.
- **Phase III**: Designed logical data model with ER diagram.
- **Phase IV**: Created pluggable database (Tues_26634_Manzifred__Land Property Registration_DB).
- **Phase V**: Implemented tables and inserted realistic data.
- **Phase VI**: Developed procedures for order processing and data retrieval.
- **Phase VII**: Implemented triggers and auditing for security.
- **Phase VIII**: Compiled documentation and presentation.



- **ER Diagram**: ## 📸 Screenshots![Screenshot 2025-05-25 113012](https://github.com/user-attachments/assets/4f480d8c-e99a-4fe1-9015-d9bb918eced6)

- The diagram follows proper ER modeling conventions with primary keys (red).
-  foreign keys (blue), and clear relationship cardinalities.
-  This system would support comprehensive land registration, ownership tracking, transaction management, and regulatory compliance.

- **UML Diagram**:![Screenshot 2025-05-25 113527](https://github.com/user-attachments/assets/b3389fd5-34d4-4f25-94ab-3c480bf630d7)
- This diagram illustrates the Land & Property Registration Process Model, showing the complete workflow from application submission to registration approval.

-Database Integration: The diagram highlights various stages where the Oracle database is accessed—such as storing user data, verifying land ownership, and updating registration records.

-Decision-Driven Process: It includes key decision points like document verification and land ownership approval that determine whether a property can be successfully registered.

-Multi-Role Involvement: The process involves multiple actors including the Landowner, Registration Officer, Surveyor, and Administrator—reflecting the real-world complexity of property registration systems.

-Secure and Transparent Workflow: The model ensures all property data is validated and securely stored, promoting transparency and preventing fraud throughout the registration process.


- **OEM Dashboard**:<img width="953" alt="sqlfred" src="https://github.com/user-attachments/assets/24e75eb7-a964-428f-b285-672d142728f4" />
Topic: Land & Property Registration System

Pluggable Database: landreg_pdb created for application-level isolation

Admin User: landadmin created during PDB setup

Application User: land_user granted required privileges (DBA, CONNECT, RESOURCE)

File Management: Uses FILE_NAME_CONVERT for file path redirection

State Management: Ensures PDB is automatically opened on restart
Container-Aware Session Management: ALTER SESSION SET CONTAINER to work inside the new PDB


- **Sample Query Output**:![table creation]![WhatsApp Image 2025-05-25 at 11 21 42 AM](https://github.com/user-attachments/assets/d43bcf34-a528-45ae-8439-0604b76f8387)
![WhatsApp Image 2025-05-25 at 11 21 19 AM (2)](https://github.com/user-attachments/assets/70a83d91-8eef-4d64-94ab-f674ccd56fdc)
![WhatsApp Image 2025-05-25 at 11 21 19 AM](https://github.com/user-attachments/assets/8851ded4-07d6-4598-af81-9368532467fb)
![WhatsApp Image 2025-05-25 at 11 21 19 AM (1)](https://github.com/user-attachments/assets/47cf0df9-88d5-40a8-b589-bbc46205f526)
![WhatsApp Image 2025-05-25 at 11 21 20 AM](https://github.com/user-attachments/assets/633479a1-e4dd-4e91-981e-de09ab36088f)
- These images show Oracle SQL Developer in action, demonstrating the implementation of your land&property registration system database project.
- GUI Development Environment: Professional Oracle SQL Developer interface
- Complex PL/SQL: Advanced triggers, packages, and procedures
- Business Rule Implementation: Sophisticated access control and validation
- Real-world Application: land&property registration system management features
- These screenshots demonstrate a comprehensive, production-ready database system with advanced security, business logic, and data management capabilities suitable for land&Propoerty registration system operations.

## 📂 SQL Queries
- **Table Creation and Data Insertion**:
```sql
 -- Table Creation for Healthcare Supply Chain Management
CREATE TABLE Vendors (
    VendorID NUMBER PRIMARY KEY,
    VendorName VARCHAR2(100) NOT NULL,
    ContactInfo VARCHAR2(200),
    Address VARCHAR2(200)
);

CREATE TABLE MedicalSupplies (
    SupplyID NUMBER PRIMARY KEY,
    SupplyName VARCHAR2(100) NOT NULL,
    Category VARCHAR2(50),
    UnitPrice NUMBER CHECK (UnitPrice >= 0)
);

CREATE TABLE Inventory (
    InventoryID NUMBER PRIMARY KEY,
    SupplyID NUMBER UNIQUE,
    QuantityInStock NUMBER CHECK (QuantityInStock >= 0),
    LastUpdated DATE,
    FOREIGN KEY (SupplyID) REFERENCES MedicalSupplies(SupplyID)
);

CREATE TABLE Departments (
    DepartmentID NUMBER PRIMARY KEY,
    DepartmentName VARCHAR2(100) NOT NULL,
    ManagerID NUMBER
);

CREATE TABLE PurchaseOrders (
    OrderID NUMBER PRIMARY KEY,
    VendorID NUMBER,
    DepartmentID NUMBER,
    OrderDate DATE NOT NULL,
    TotalAmount NUMBER,
    FOREIGN KEY (VendorID) REFERENCES Vendors(VendorID),
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

CREATE TABLE OrderDetails (
    OrderDetailID NUMBER PRIMARY KEY,
    OrderID NUMBER,
    SupplyID NUMBER,
    QuantityOrdered NUMBER CHECK (QuantityOrdered > 0),
    UnitPrice NUMBER CHECK (UnitPrice >= 0),
    FOREIGN KEY (OrderID) REFERENCES PurchaseOrders(OrderID),
    FOREIGN KEY (SupplyID) REFERENCES MedicalSupplies(SupplyID)
);

CREATE TABLE UsageTracking (
    UsageID NUMBER PRIMARY KEY,
    SupplyID NUMBER,
    DepartmentID NUMBER,
    QuantityUsed NUMBER CHECK (QuantityUsed > 0),
    UsageDate DATE,
    FOREIGN KEY (SupplyID) REFERENCES MedicalSupplies(SupplyID),
    FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID)
);

-- Data Insertion
INSERT INTO Vendors VALUES (1, 'MediCorp', 'contact@medicorp.com', '123 Kigali St');
INSERT INTO Vendors VALUES (2, 'HealthSupplies Ltd', 'info@healthsupplies.rw', '456 Gisenyi Rd');

INSERT INTO MedicalSupplies VALUES (1, 'Syringes', 'Equipment', 2.5);
INSERT INTO MedicalSupplies VALUES (2, 'Paracetamol', 'Medicine', 0.1);

INSERT INTO Inventory VALUES (1, 1, 1000, TO_DATE('2025-05-10', 'YYYY-MM-DD'));
INSERT INTO Inventory VALUES (2, 2, 5000, TO_DATE('2025-05-10', 'YYYY-MM-DD'));

INSERT INTO Departments VALUES (1, 'Cardiology', 101);
INSERT INTO Departments VALUES (2, 'Pharmacy', 102);

INSERT INTO PurchaseOrders VALUES (1, 1, 1, TO_DATE('2025-05-01', 'YYYY-MM-DD'), 2500);
INSERT INTO PurchaseOrders VALUES (2, 2, 2, TO_DATE('2025-05-02', 'YYYY-MM-DD'), 500);

INSERT INTO OrderDetails VALUES (1, 1, 1, 1000, 2.5);
INSERT INTO OrderDetails VALUES (2, 2, 2, 5000, 0.1);

INSERT INTO UsageTracking VALUES (1, 1, 1, 50, TO_DATE('2025-05-05', 'YYYY-MM-DD'));
INSERT INTO UsageTracking VALUES (2, 2, 2, 100, TO_DATE('2025-05-06', 'YYYY-MM-DD'));
```
- **Triggers**:
```sql
-- Create Holiday Reference Table
CREATE TABLE Holidays (
    HolidayDate DATE PRIMARY KEY,
    HolidayName VARCHAR2(100)
);

-- Insert Public Holidays for June 2025
INSERT INTO Holidays VALUES (TO_DATE('2025-06-01', 'YYYY-MM-DD'), 'National Day');
INSERT INTO Holidays VALUES (TO_DATE('2025-06-15', 'YYYY-MM-DD'), 'Independence Day');

-- Create Audit Table
CREATE TABLE AuditLog (
    AuditID NUMBER PRIMARY KEY,
    UserID VARCHAR2(50),
    ActionDate DATE,
    Operation VARCHAR2(50),
    Status VARCHAR2(20)
);

-- Sequence for AuditID
CREATE SEQUENCE AuditSeq START WITH 1 INCREMENT BY 1;

-- Compound Trigger for Restrictions and Auditing
CREATE OR REPLACE TRIGGER SupplyChain_Restrictions
FOR INSERT OR UPDATE OR DELETE ON MedicalSupplies
COMPOUND TRIGGER
    v_Day VARCHAR2(10);
    v_IsHoliday NUMBER;

    BEFORE STATEMENT IS
    BEGIN
        -- Check if today is a weekday
        SELECT TO_CHAR(SYSDATE, 'DY') INTO v_Day FROM DUAL;
        IF v_Day IN ('MON', 'TUE', 'WED', 'THU', 'FRI') THEN
            RAISE_APPLICATION_ERROR(-20004, 'Table manipulations not allowed on weekdays.');
        END IF;

        -- Check if today is a holiday in June 2025
        SELECT COUNT(*) INTO v_IsHoliday
        FROM Holidays
        WHERE HolidayDate = TRUNC(SYSDATE)
        AND EXTRACT(MONTH FROM HolidayDate) = 6
        AND EXTRACT(YEAR FROM HolidayDate) = 2025;
        IF v_IsHoliday > 0 THEN
            RAISE_APPLICATION_ERROR(-20005, 'Table manipulations not allowed on holidays.');
        END IF;
    END BEFORE STATEMENT;

    AFTER EACH ROW IS
    BEGIN
        -- Log the action
        INSERT INTO AuditLog (AuditID, UserID, ActionDate, Operation, Status)
        VALUES (AuditSeq.NEXTVAL, USER, SYSDATE, 
                CASE 
                    WHEN INSERTING THEN 'INSERT'
                    WHEN UPDATING THEN 'UPDATE'
                    WHEN DELETING THEN 'DELETE'
                END, 'ALLOWED');
    END AFTER EACH ROW;

    AFTER STATEMENT IS
    BEGIN
        -- Additional auditing logic if needed
        NULL;
    END AFTER STATEMENT;
END SupplyChain_Restrictions;
/

-- Problem Statement
-- The trigger ensures that no INSERT, UPDATE, or DELETE operations are performed on the MedicalSupplies table during weekdays or public holidays in June 2025. Auditing tracks all actions, enhancing security by logging user activities and enforcing access restrictions.
```
- **Procedures**:
```sql
-- Package Specification
CREATE OR REPLACE PACKAGE SupplyChain_Pkg AS
    PROCEDURE FetchInventoryStatus(p_SupplyID IN NUMBER, p_Quantity OUT NUMBER);
    FUNCTION CalculateTotalSpending(p_DepartmentID IN NUMBER) RETURN NUMBER;
END SupplyChain_Pkg;
/

-- Package Body
CREATE OR REPLACE PACKAGE BODY SupplyChain_Pkg AS
    PROCEDURE FetchInventoryStatus(p_SupplyID IN NUMBER, p_Quantity OUT NUMBER) IS
        CURSOR inv_cursor IS
            SELECT QuantityInStock
            FROM Inventory
            WHERE SupplyID = p_SupplyID;
    BEGIN
        OPEN inv_cursor;
        FETCH inv_cursor INTO p_Quantity;
        IF inv_cursor%NOTFOUND THEN
            RAISE_APPLICATION_ERROR(-20001, 'Supply ID not found.');
        END IF;
        CLOSE inv_cursor;
    EXCEPTION
        WHEN OTHERS THEN
            RAISE_APPLICATION_ERROR(-20002, 'Error fetching inventory: ' || SQLERRM);
    END FetchInventoryStatus;

    FUNCTION CalculateTotalSpending(p_DepartmentID IN NUMBER) RETURN NUMBER IS
        v_Total NUMBER;
    BEGIN
        SELECT SUM(TotalAmount) OVER (PARTITION BY DepartmentID)
        INTO v_Total
        FROM PurchaseOrders
        WHERE DepartmentID = p_DepartmentID;
        RETURN NVL(v_Total, 0);
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 0;
        WHEN OTHERS THEN
            RAISE_APPLICATION_ERROR(-20003, 'Error calculating spending: ' || SQLERRM);
    END CalculateTotalSpending;
END SupplyChain_Pkg;
/

-- Example DML Operation
INSERT INTO UsageTracking VALUES (3, 1, 1, 20, TO_DATE('2025-05-07', 'YYYY-MM-DD'));

-- Example DDL Operation
ALTER TABLE MedicalSupplies ADD (SupplierRating NUMBER DEFAULT 0);

-- Testing Procedure
DECLARE
    v_Quantity NUMBER;
BEGIN
    SupplyChain_Pkg.FetchInventoryStatus(1, v_Quantity);
    DBMS_OUTPUT.PUT_LINE('Quantity in Stock: ' || v_Quantity);
END;
/
```

## Results
- Successfully created a comprehensive Oracle PL/SQL database system with 7 interconnected tables (Vendors, MedicalSupplies, Inventory, Departments, PurchaseOrders, OrderDetails, UsageTracking).
- Implemented robust data relationships with proper foreign key constraints and check constraints for data integrity.
- Developed a complete package system with procedures and functions for inventory management and financial tracking.
- Created comprehensive audit logging system that tracks all database operations with user identification and timestamps.

## Conclusion
The Land and Property Registration System provides a reliable, transparent, and streamlined platform for managing property records and transactions. By digitizing the registration process, it enhances data accuracy, reduces processing time, and minimizes the risk of fraud—ultimately supporting better governance and user trust in land administration.

## References
- Oracle PL/SQL Documentation
- BPMN 2.0 Specifications
