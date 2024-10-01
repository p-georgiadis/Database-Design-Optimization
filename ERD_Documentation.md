# Entity-Relationship Diagram (ERD) Documentation for "Books & Brews"

## Overview
This document provides a detailed overview of the entities and relationships involved in the "Books & Brews" bookstore and café business database. The ERD and subsequent relational model were developed to support various business operations, including tracking customer purchases, managing inventory, and handling events. The ERD is represented using Crow's Foot notation, and each entity is defined with its attributes and relationships to other entities.

## Entities and Relationships

### Customer
| Field             | Type       | Description                                        |
|-------------------|------------|----------------------------------------------------|
| Customer_ID (PK)  | INT        | Primary key, unique ID for each customer            |
| First_Name        | VARCHAR    | Customer's first name                              |
| Last_Name         | VARCHAR    | Customer's last name                               |
| Email             | VARCHAR    | Customer's email address                           |
| Phone             | VARCHAR    | Customer's phone number                            |
| Membership_Status | BOOLEAN    | Indicates if the customer is a member (true/false) |

**Relationships:**
- A customer can have multiple transactions (`1:M` relationship with Transaction).
- A customer can attend multiple events (`M:N` relationship with Event through Event_Attendance).

### Book
| Field             | Type       | Description                                        |
|-------------------|------------|----------------------------------------------------|
| ISBN (PK)         | VARCHAR    | Primary key, unique identifier for each book       |
| Title             | VARCHAR    | Book title                                         |
| Author            | VARCHAR    | Name of the book author                            |
| Genre             | VARCHAR    | Category or genre of the book                      |
| Year_of_Publication | INT      | Year the book was published                        |
| Price             | DECIMAL    | Price of the book                                  |

**Relationships:**
- Books can be part of multiple transactions (`M:N` relationship with Transaction via Transaction_Detail).
- Books are linked to the Inventory to manage stock levels (`1:1` relationship with Inventory).

### Café Item
| Field           | Type       | Description                                        |
|-----------------|------------|----------------------------------------------------|
| Item_ID (PK)    | INT        | Primary key, unique ID for each café item          |
| Name            | VARCHAR    | Name of the café item                              |
| Category        | VARCHAR    | Category of the item (e.g., Beverage, Pastry)      |
| Price           | DECIMAL    | Price of the café item                             |

**Relationships:**
- Café Items can be part of multiple transactions (`M:N` relationship with Transaction via Transaction_Detail).
- Café Items are linked to the Inventory to manage stock levels (`1:1` relationship with Inventory).

### Employee
| Field           | Type       | Description                                        |
|-----------------|------------|----------------------------------------------------|
| Employee_ID (PK)| INT        | Primary key, unique ID for each employee           |
| First_Name      | VARCHAR    | Employee's first name                              |
| Last_Name       | VARCHAR    | Employee's last name                               |
| Role            | VARCHAR    | Role of the employee (e.g., Cashier, Barista)      |
| Shift_Schedule  | VARCHAR    | Assigned work schedule                             |

**Relationships:**
- Employees handle transactions (`1:M` relationship with Transaction).
- Employees are assigned to manage events (`1:M` relationship with Event).

### Transaction
| Field             | Type       | Description                                        |
|-------------------|------------|----------------------------------------------------|
| Transaction_ID (PK)| INT       | Primary key, unique ID for each transaction        |
| Date              | DATE       | Date of the transaction                            |
| Total_Amount      | DECIMAL    | Total amount of the transaction                    |
| Customer_ID (FK)  | INT        | Foreign key, references Customer                   |
| Employee_ID (FK)  | INT        | Foreign key, references Employee                   |

**Relationships:**
- A transaction can include multiple books and café items (`M:N` relationship with Book and Café Item through Transaction_Detail).

### Transaction_Detail
| Field             | Type       | Description                                        |
|-------------------|------------|----------------------------------------------------|
| Transaction_ID (FK)| INT       | Foreign key, references Transaction                |
| ISBN (FK)         | VARCHAR    | Foreign key, references Book                       |
| Item_ID (FK)      | INT        | Foreign key, references Café Item                  |
| Quantity          | INT        | Number of books or café items purchased            |

**Relationships:**
- `Transaction_Detail` links `Transaction` with `Book` and `Café Item` through a `M:N` relationship.

### Event
| Field             | Type       | Description                                        |
|-------------------|------------|----------------------------------------------------|
| Event_ID (PK)     | INT        | Primary key, unique ID for each event              |
| Name              | VARCHAR    | Event name                                         |
| Date              | DATE       | Date of the event                                  |
| Description       | TEXT       | Description of the event                           |
| Employee_ID (FK)  | INT        | Foreign key, references Employee                   |

**Relationships:**
- Events are managed by employees (`1:M` relationship with Employee).
- Customers can RSVP and attend events (`M:N` relationship with Customer through Event_Attendance).

### Event_Attendance
| Field             | Type       | Description                                        |
|-------------------|------------|----------------------------------------------------|
| Event_ID (FK)     | INT        | Foreign key, references Event                      |
| Customer_ID (FK)  | INT        | Foreign key, references Customer                   |

**Relationships:**
- `Event_Attendance` links `Event` with `Customer` through a `M:N` relationship.

### Inventory
| Field             | Type       | Description                                        |
|-------------------|------------|----------------------------------------------------|
| Item_ID/ISBN (PK) | VARCHAR    | Primary key, references either Book or Café Item   |
| Quantity          | INT        | Current stock level                                |
| Restock_Date      | DATE       | Date when the item was last restocked              |

**Relationships:**
- Inventory is managed separately for books and café items (`1:1` relationship with Book and Café Item).

## ERD in ASCII (Text-Based Representation)
```plaintext
+--------------------+     +-------------------+     +---------------------+
|    Customer        |     |     Transaction   |     |    Employee         |
|--------------------|     |-------------------|     |---------------------|
| Customer_ID (PK)   | <-- | Transaction_ID (PK)| --> | Employee_ID (PK)    |
| First_Name         |     | Date              |     | First_Name          |
| Last_Name          |     | Total_Amount      |     | Last_Name           |
| Email              |     | Customer_ID (FK)  |     | Role                |
| Phone              |     | Employee_ID (FK)  |     | Shift_Schedule      |
| Membership_Status  |     +-------------------+     +---------------------+
+--------------------+     

+------------------+       +---------------------+      +-----------------------+
|      Book        | <---> |   Transaction_Detail| ---> |      Café Item        |
|------------------|       |---------------------|      |-----------------------|
| ISBN (PK)        |       | Transaction_ID (FK) |      | Item_ID (PK)          |
| Title            |       | ISBN (FK)           |      | Name                  |
| Author           |       | Item_ID (FK)        |      | Category              |
| Genre            |       | Quantity            |      | Price                 |
| Year_of_Pub.     |       +---------------------+      +-----------------------+
| Price            |
+------------------+

+-----------------------+       +---------------------+
|         Event         | <---> |   Event_Attendance  |
|-----------------------|       |---------------------|
| Event_ID (PK)         |       | Event_ID (FK)       |
| Name                  |       | Customer_ID (FK)    |
| Date                  |       +---------------------+
| Description           |
| Employee_ID (FK)      |
+-----------------------+

+----------------------+
|      Inventory       |
|----------------------|
| Item_ID/ISBN (PK)    |
| Quantity             |
| Restock_Date         |
+----------------------+
```

## Diagram Summary
The above tables and their relationships have been modeled to capture the key functionalities of "Books & Brews." Each table is normalized and linked using primary and foreign keys to maintain data integrity and support complex queries and reporting. The ERD illustrates how entities like `Customer`, `Transaction`, `Book`, and `Employee` interact, providing a visual representation of how data flows in the database.

This documentation serves as a blueprint for database implementation, ensuring all business requirements are met with a well-structured and efficient relational database model.
