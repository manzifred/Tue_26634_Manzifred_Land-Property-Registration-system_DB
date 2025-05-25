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
 -- -- Land & Property Registration System Database Schema

CREATE TABLE Owners (
    OwnerID NUMBER PRIMARY KEY,
    FullName VARCHAR2(100) NOT NULL,
    NationalID VARCHAR2(16) UNIQUE,
    PhoneNumber VARCHAR2(20),
    Email VARCHAR2(100)
);

CREATE TABLE Properties (
    PropertyID NUMBER PRIMARY KEY,
    OwnerID NUMBER,
    PropertyType VARCHAR2(50),
    Location VARCHAR2(200),
    SizeInSqMeters NUMBER CHECK (SizeInSqMeters > 0),
    RegistrationDate DATE,
    FOREIGN KEY (OwnerID) REFERENCES Owners(OwnerID)
);

CREATE TABLE Districts (
    DistrictID NUMBER PRIMARY KEY,
    DistrictName VARCHAR2(100) NOT NULL,
    Region VARCHAR2(100)
);

CREATE TABLE PropertyTransfer (
    TransferID NUMBER PRIMARY KEY,
    PropertyID NUMBER,
    FromOwnerID NUMBER,
    ToOwnerID NUMBER,
    TransferDate DATE,
    TransferReason VARCHAR2(200),
    FOREIGN KEY (PropertyID) REFERENCES Properties(PropertyID),
    FOREIGN KEY (FromOwnerID) REFERENCES Owners(OwnerID),
    FOREIGN KEY (ToOwnerID) REFERENCES Owners(OwnerID)
);

CREATE TABLE LandValuation (
    ValuationID NUMBER PRIMARY KEY,
    PropertyID NUMBER,
    AssessedValue NUMBER CHECK (AssessedValue >= 0),
    ValuationDate DATE,
    FOREIGN KEY (PropertyID) REFERENCES Properties(PropertyID)
);

CREATE TABLE RegistrationAudit (
    AuditID NUMBER PRIMARY KEY,
    UserName VARCHAR2(50),
    Operation VARCHAR2(20),
    TableAffected VARCHAR2(50),
    OperationDate DATE,
    Status VARCHAR2(10)
);

CREATE TABLE Holidays (
    HolidayID NUMBER PRIMARY KEY,
    HolidayDate DATE UNIQUE,
    Description VARCHAR2(100)
);

```
- **Triggers**:
```sql
CREATE OR REPLACE TRIGGER trg_prevent_weekend_holiday_insert
BEFORE INSERT ON land_registration
FOR EACH ROW
DECLARE
    v_day VARCHAR2(10);
    v_holiday_count NUMBER;
BEGIN
    -- Get the day of the week
    SELECT TO_CHAR(:NEW.registration_date, 'DAY') INTO v_day FROM dual;

    -- Check if it's Saturday or Sunday
    IF TRIM(UPPER(v_day)) IN ('SATURDAY', 'SUNDAY') THEN
        RAISE_APPLICATION_ERROR(-20001, 'Insert not allowed on weekends.');
    END IF;

    -- Check if the date is in the HOLIDAYS table
    SELECT COUNT(*) INTO v_holiday_count
    FROM holidays
    WHERE holiday_date = TRUNC(:NEW.registration_date);

    IF v_holiday_count > 0 THEN
        RAISE_APPLICATION_ERROR(-20002, 'Insert not allowed on public holidays.');
    END IF;
END;
-- Audit Table
CREATE TABLE property_delete_audit (
    property_id       NUMBER,
    deleted_by        VARCHAR2(50),
    deleted_at        DATE,
    reason            VARCHAR2(200)
);

-- Compound Trigger
CREATE OR REPLACE TRIGGER trg_audit_property_deletion
FOR DELETE ON property
COMPOUND TRIGGER
    TYPE t_property IS RECORD (
        id property.property_id%TYPE,
        username VARCHAR2(50)
    );
    TYPE t_property_list IS TABLE OF t_property;
    v_deleted_properties t_property_list := t_property_list();

    BEFORE EACH ROW IS
    BEGIN
        v_deleted_properties.EXTEND;
        v_deleted_properties(v_deleted_properties.COUNT).id := :OLD.property_id;
        v_deleted_properties(v_deleted_properties.COUNT).username := USER;
    END BEFORE EACH ROW;

    AFTER STATEMENT IS
    BEGIN
        FOR i IN 1 .. v_deleted_properties.COUNT LOOP
            INSERT INTO property_delete_audit
            VALUES (
                v_deleted_properties(i).id,
                v_deleted_properties(i).username,
                SYSDATE,
                'Manual deletion'
            );
        END LOOP;
    END AFTER STATEMENT;
END;
-- Audit Table
CREATE TABLE audit_log (
    user_id     VARCHAR2(30),
    action_time DATE,
    operation   VARCHAR2(20),
    table_name  VARCHAR2(50),
    status      VARCHAR2(10)
);

-- Trigger
CREATE OR REPLACE TRIGGER trg_audit_sensitive_update
BEFORE UPDATE ON land_owner
FOR EACH ROW
DECLARE
    v_user VARCHAR2(30) := USER;
    v_status VARCHAR2(10) := 'ALLOWED';
BEGIN
    -- Restrict unauthorized users
    IF v_user NOT IN ('ADMIN', 'LAND_OFFICER') THEN
        v_status := 'DENIED';

        INSERT INTO audit_log (user_id, action_time, operation, table_name, status)
        VALUES (v_user, SYSDATE, 'UPDATE', 'land_owner', v_status);

        RAISE_APPLICATION_ERROR(-20003, 'You are not authorized to update landowner records.');
    END IF;

    -- If allowed, log the operation
    INSERT INTO audit_log (user_id, action_time, operation, table_name, status)
    VALUES (v_user, SYSDATE, 'UPDATE', 'land_owner', v_status);
END;

```
- **Procedures**:
```sql
CREATE OR REPLACE PACKAGE LandRegistry_Pkg AS
    PROCEDURE FetchPropertySize(p_PropertyID IN NUMBER, p_Size OUT NUMBER);
    FUNCTION CalculateTotalValueByOwner(p_OwnerID IN NUMBER) RETURN NUMBER;
END LandRegistry_Pkg;
/
CREATE OR REPLACE PACKAGE BODY LandRegistry_Pkg AS

    -- Procedure to get property size
    PROCEDURE FetchPropertySize(p_PropertyID IN NUMBER, p_Size OUT NUMBER) IS
        CURSOR size_cursor IS
            SELECT SizeInSqMeters
            FROM Properties
            WHERE PropertyID = p_PropertyID;
    BEGIN
        OPEN size_cursor;
        FETCH size_cursor INTO p_Size;
        IF size_cursor%NOTFOUND THEN
            RAISE_APPLICATION_ERROR(-20010, 'Property not found.');
        END IF;
        CLOSE size_cursor;
    EXCEPTION
        WHEN OTHERS THEN
            RAISE_APPLICATION_ERROR(-20011, 'Error fetching property size: ' || SQLERRM);
    END FetchPropertySize;

    -- Function to calculate total valuation for an owner
    FUNCTION CalculateTotalValueByOwner(p_OwnerID IN NUMBER) RETURN NUMBER IS
        v_TotalValue NUMBER;
    BEGIN
        SELECT SUM(LV.AssessedValue)
        INTO v_TotalValue
        FROM LandValuation LV
        JOIN Properties P ON LV.PropertyID = P.PropertyID
        WHERE P.OwnerID = p_OwnerID;

        RETURN NVL(v_TotalValue, 0);
    EXCEPTION
        WHEN NO_DATA_FOUND THEN
            RETURN 0;
        WHEN OTHERS THEN
            RAISE_APPLICATION_ERROR(-20012, 'Error calculating total value: ' || SQLERRM);
    END CalculateTotalValueByOwner;

END LandRegistry_Pkg;
/
-- Test the FetchPropertySize procedure
DECLARE
    v_Size NUMBER;
BEGIN
    LandRegistry_Pkg.FetchPropertySize(1, v_Size);
    DBMS_OUTPUT.PUT_LINE('Size in Square Meters: ' || v_Size);
END;
/

-- Test the CalculateTotalValueByOwner function
DECLARE
    v_Value NUMBER;
BEGIN
    v_Value := LandRegistry_Pkg.CalculateTotalValueByOwner(1);
    DBMS_OUTPUT.PUT_LINE('Total Assessed Property Value: ' || v_Value);
END;
/
-- Insert new land valuation
INSERT INTO LandValuation VALUES (3, 1, 18000000, TO_DATE('2025-05-25', 'YYYY-MM-DD'));
-- Add new column to track registration status
ALTER TABLE Properties ADD (RegistrationStatus VARCHAR2(20) DEFAULT 'Pending');

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
