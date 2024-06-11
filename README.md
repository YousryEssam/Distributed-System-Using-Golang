# Distributed System Project using Golang

## Overview
This project is a simple distributed banking system implemented in Golang. The system consists of a master server and multiple slave clients. The master server handles database operations, while slave clients connect to the master to perform various banking operations such as login, account creation, balance inquiry, deposits, and withdrawals.

## Features
- **Account Management**: Create a new account with an account number, customer ID, password, and account type.
- **Login**: Secure login using account number and password.
- **Balance Inquiry**: Check the balance of the account.
- **Deposits and Withdrawals**: Perform deposit and withdrawal operations.

## Project Structure
- `master.go`: Master server code that handles database connections and client requests.
- `slave.go`: Slave client code that interacts with the master server to perform banking operations.
- `database.sql`: SQL script to set up the required database and tables.

## Getting Started

### Prerequisites
- Go (Golang) installed on your machine.
- MySQL database installed and running.

  # Requirements for Distributed System Project using Golang

## Functional Requirements

### Master Server
1. **Database Connection**: The master server must connect to a MySQL database.
2. **Listening for Connections**: The master server must listen for incoming TCP connections from slave clients.
3. **Handling Requests**: The master server must handle various requests from slave clients:
   - **Login**: Validate account number and password.
   - **Account Creation**: Create a new account with provided details.
   - **Balance Inquiry**: Retrieve the balance for a given account.
   - **Deposit**: Update the balance for a given account with the deposited amount.
   - **Withdrawal**: Update the balance for a given account after withdrawal.
4. **Database Operations**: The master server must perform necessary database operations for each request, including:
   - Inserting and retrieving records from `Customer`, `Account`, and `Transaction` tables.
   - Updating account balances.

### Slave Client
1. **User Interaction**: The slave client must provide a user interface to interact with the banking system.
2. **Connection to Master**: The slave client must connect to the master server via TCP.
3. **Sending Requests**: The slave client must send requests to the master server based on user input:
   - **Login**: Send account number and password to the master server.
   - **Account Creation**: Send new account details to the master server.
   - **Balance Inquiry**: Request balance for the logged-in account.
   - **Deposit**: Send deposit amount to the master server.
   - **Withdrawal**: Send withdrawal amount to the master server.
4. **Receiving Responses**: The slave client must handle responses from the master server and display appropriate messages to the user.

## Non-Functional Requirements

1. **Security**: 
   - Passwords should be securely stored and transmitted.
   - Sensitive data should be handled appropriately to prevent unauthorized access.

2. **Performance**:
   - The system should handle multiple simultaneous connections efficiently.
   - Database queries should be optimized for performance.

3. **Reliability**:
   - The system should handle and recover from errors gracefully.
   - Connections between master and slave should be reliable and robust.

4. **Scalability**:
   - The system should be able to scale with an increasing number of slave clients.
   - The database should handle a growing number of records without performance degradation.

5. **Maintainability**:
   - The codebase should be modular and well-documented.
   - The system should be easy to maintain and extend.

## Technical Requirements

1. **Programming Language**: Go (Golang)
2. **Database**: MySQL
3. **Libraries**:
   - `database/sql`: For database operations.
   - `net`: For network operations.
   - `github.com/go-sql-driver/mysql`: MySQL driver for Go.

## Environment Setup

1. **Go Installation**: Ensure Go is installed on your machine. Follow the installation guide [here](https://golang.org/doc/install).
2. **MySQL Installation**: Install MySQL and ensure it is running. Follow the installation guide [here](https://dev.mysql.com/doc/mysql-installation-excerpt/5.7/en/).
3. **Database Setup**: Run the `database.sql` script to set up the required database and tables.

## Running the Project

1. **Start the Master Server**:
    ```sh
    go run master.go
    ```

2. **Start the Slave Client**:
    ```sh
    go run slave.go
    ```

3. **Interaction**: Follow the prompts on the slave client to interact with the system.

## Testing

1. **Unit Testing**: Write unit tests for individual components to ensure they work as expected.
2. **Integration Testing**: Test the interaction between master server and slave clients.
3. **Performance Testing**: Ensure the system performs well under load.

## Documentation

1. **Code Comments**: Ensure all major functions and components are well-documented with comments.
2. **User Guide**: Provide a user guide for setting up and using the system.
3. **API Documentation**: Document the communication protocol between master server and slave clients.

## Contribution Guidelines

1. **Code Style**: Follow standard Go code style guidelines.
2. **Pull Requests**: Submit pull requests for any enhancements or bug fixes.
3. **Issue Tracking**: Use the issue tracker to report bugs or suggest features.

---

This document outlines the requirements for building and running the distributed banking system using Golang. Ensure all functional and non-functional requirements are met to deliver a robust and efficient system.

### Installation

1. **Clone the repository:**
    ```sh
    git clone https://github.com/yourusername/distributed-system-golang.git
    cd distributed-system-golang
    ```

2. **Set up the MySQL Database:**
    - Start your MySQL server.
    - Create the `BankATM` database and tables by running the SQL commands provided in `database.sql`.

3. **Configure the Database Connection:**
    - Edit the `master.go` file to update the MySQL connection string with your database credentials.
      ```go
      db, err = sql.Open("mysql", "root:yourpassword@tcp(localhost:3306)/BankATM")
      ```

### Usage

1. **Run the Master Server:**
    ```sh
    go run master.go
    ```

2. **Run the Slave Client:**
    ```sh
    go run slave.go
    ```

3. **Interact with the Slave Client:**
    - Follow the on-screen prompts to login, create an account, check balance, deposit, or withdraw funds.

## Example Interaction
--------------------------Welcome to ABC Bank-------------------------------
--->> Press 1 to login to your account.
--->> Press 2 to Create an account.
--->> Press 3 to Shutdown The System.
--->> Please Enter your account number :
--->> Please Enter your account password :
--------------------Successful login, welcome back.-------------------------
--->> Please Select what you want to do.
--->> Press 1 to see you balance.
--->> Press 2 to deposit.
--->> Press 3 to withdraw.
--->> Press on any other key to quit.

## Database Schema

### Customer Table
```sql
CREATE TABLE Customer (
    CustomerID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerName VARCHAR(100),
    Address VARCHAR(255),
    PhoneNumber VARCHAR(20),
    DateOfBirth DATE
);
```
###Account Table
```sql
CREATE TABLE Account (
    AccountNumber CHAR(16) PRIMARY KEY CHECK (AccountNumber REGEXP '^[0-9]{16}$'),   
    CustomerID INT,
    AccountPassword VARCHAR(4),
    Balance DECIMAL(10, 3),
    AccountType VARCHAR(50),
    DateOpened DATE,
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),
    CONSTRAINT Check_AccountNumber_Length CHECK (LENGTH(AccountNumber) = 16)
);
```

###Transaction Table
```sql
CREATE TABLE Transaction (
    TransactionID INT AUTO_INCREMENT PRIMARY KEY,
    AccountNumber CHAR(16) CHECK (AccountNumber REGEXP '^[0-9]{16}$'),   
    TransactionType VARCHAR(50),
    Amount DECIMAL(10, 3),
    NewBalance DECIMAL(10, 3),
    TransactionDate DATETIME,
    FOREIGN KEY (AccountNumber) REFERENCES Account(AccountNumber)
);
```

