# banking_system

The Banking Management System is a simple SQL-based project developed to simulate core banking operations. It allows managing customers, accounts, and transactions while demonstrating practical use of relational databases, SQL queries, stored procedures, and constraints.

Key Objectives

Customer Management: Efficiently store and manage customer information, ensuring data integrity and security.

##Account Management: Enable creation, modification, and closure of bank accounts with proper validations.

##Transaction Handling: Facilitate deposits, withdrawals, and fund transfers with accurate recording and balance checks.
Advantages

Data Management: Centralized storage of customer and account information.

Accuracy: Automated calculations reduce human error in transactions.

Efficiency: Quick processing of deposits, withdrawals, and transfers.

Transparency: Maintains detailed transaction history for auditing.

Learning Tool: Provides hands-on practice with SQL, stored procedures, and relational database design.

Disadvantages

Limited Interface: Works mainly via MySQL CLI or Workbench; no GUI for end-users.

Scalability: Not designed for large-scale banking operations.

Security: Lacks advanced authentication and encryption features.

Manual Maintenance: Requires manual script execution for some operations.

Working Flow

Customer Management: Add or update customer details.

Account Creation: Link new accounts to customers with initial balance.

Transactions: Perform deposits, withdrawals, and transfers.

Transaction Logging: Record each transaction in the transactions table.

Balance Check: Validate account balance before withdrawal or transfer.

Reporting: Generate statements or view transaction history.

- Step 1: Create and use database
CREATE DATABASE bank; USE bank;
CREATE TABLE acc_table ( account_number INT PRIMARY KEY, customer_name VARCHAR(30) NOT NULL, gender ENUM('male', 'female', 'other') NOT NULL, account_type ENUM('saving', 'current') NOT NULL DEFAULT 'saving', current_balance INT NOT NULL );

-- Step 3: Insert Sample Data
INSERT INTO acc_table VALUES
(890673, 'smith', 'male', 'saving', 10000.49),(809620, 'akash', 'male', 'saving', 72800.19),(880903, 'ramesh', 'male', 'current', 21000.49),(867456, 'prakash', 'male', 'current', 52500.64),(780673, 'ganesh', 'male', 'saving', 20600.44),(823121, 'Laksh', 'male', 'saving', 45000.00),(790632, 'rakshita', 'female', 'saving', 28940.10),(694603, 'pooja', 'female', 'current', 70000.00),(934565, 'reshma', 'female', 'current', 63200.80),(345638, 'reshma', 'female', 'current', 63200.80,(870635, 'shejal', female, 'current', 44900.10),(743631, 'priya', 'female', 'saving', 86100.26),(690644, 'soni',' female', 'current', 34000.12),(790633, 'raju', 'male', 'saving', 60000.12),(653412, 'neha', 'female', 'current', 25000.00);

-- Step 4: Create Transaction Table
CREATE TABLE transaction_table ( trans_id INT PRIMARY KEY, account_number INT, trans_type ENUM('deposit','withdrawal','transfer') NOT NULL, amount INT NOT NULL, trans_date DATE NOT NULL, FOREIGN KEY (account_number) REFERENCES account(account_number) );

-- Step 5: Insert Sample Transactions
INSERT INTO transaction  VALUES 
(12345,809620,  'withdraw', 72800, '2025-11-22'), (45678,780673, 'withdraw', 20600, '2025-11-22'), (46751,690644,  'withdraw', 34000, '2025-11-22'), (54273, 743631,  'deposit', 86100, '2025-11-22'), (54324, 880903,  'deposite', 21000, '2025-11-22'), (56324, 867456,  'withdraw', 52500, '2025-11-22'), (63627, 694603,  'deposite', 70000, '2025-11-22'), (64329, 934565 'deposit', 63200, '2025-11-22'), (72599, 823121,  'deposite', 45000, '2025-11-22'), (86024, 653412,  'deposit', 25000, '2025-11-22'), (86182, 790633,  'deposite', 60000, '2025-11-22'), (86905, 870635,  'withdraw', 44900, '2025-11-10'),(94321, 345658, 'withdraw', 63200),(57891, 790632, 'withdraw', 28940);

-- Step 6: Queries with Output
-- 1️⃣ Retrieve all customers with balance > 50,000
select customer_name from acc_table WHERE current_balance>50,000;

-- 2️⃣ Display account number, customer name, and account type of all SAVING accounts
SELECT account_number, customer_name, account_type FROM acc_table WHERE account_type = 'saving';

-- 3️⃣ List all transactions made in the current month
SELECT * FROM transaction_tablew WHERE MONTH(transaction_date) = MONTH(CURDATE_DATE()) AND YEAR(transaction_date) = YEAR(CURDATE_DATE());

-- 4️⃣ Show customers who have not made any transactions yet
select customer_name from acc_table left join transaction_table on acc_table.account_number=transaction_table.account_number WHERE transaction_id is null;

-- 5️⃣ Display the top 3 customers with the highest account balance
select customer_name,current_balance from acc_table order by current_balance desc limit 3;

-- 6️⃣ Retrieve all transactions where the amount is greater than 10,000
select * from transaction_table WHERE amount>10,000;

-- 7️⃣ Show the total balance of all accounts combined
select sum(current_balance)as total_balance from acc_table;

-- 8️⃣ List customers along with their total deposited amount
select customer_name,sum(amount)as total_amount from acc_table inner join transaction_table on acc_table.account_number=transaction_table.account_number WHERE transaction_type='deposite' group by customer_name;

-- 9️⃣ Find customers who made a withdrawal of more than 5,000
select customer_name from acc_table inner join transaction_table on acc_table.account_number=transaction_table.account_number WHERE transaction_type='withdraw' and amount>5000;

-- 🔟 Display the most recent transaction date for each account
select account_number,max(transaction_date)as most_recent_transaction from transaction_table group by account_number;

-- 1️⃣1️⃣ Retrieve the number of transactions each customer has made
select account_number,customer_name,count(transaction_id)as total_transaction from acc_table left join transaction_table on acc_table.account_number=transaction_table.account_number group by account_number,customer_name;

-- 1️⃣2️⃣ List customers who have both SAVINGS and CURRENT accounts
select customer_name from acc_table inner join transaction_table on acc_table.account_number=transaction_table.account_number WHERE account_type in('saving','current') group by customer_name;

--13 Find allaccounts created by customers whose name starts with 'P'.
select * from acc_table inner join transaction_table on acc_table.account_number=transaction_table.account_number WHERE customer_name like'P%';

 -- 14 Retrieve customers sorted by their account balance in descending order
select *nfrom acc_table order by current_balance desc;

-- 15 Display the average account balance per account type
select account_type,avg(current_balance)as avg_balance from acc_table group by account_type;

Conclusion

The Banking Management System demonstrates fundamental banking operations using MySQL, providing a practical approach to managing customers, accounts, and transactions. It emphasizes database design, relational integrity, and the use of stored procedures for automation. While it is suitable for learning and small-scale simulations, it highlights the importance of accuracy, transparency, and structured data management in real-world banking. This project serves as a solid foundation for building more advanced banking applications with enhanced security, scalability, and user interfaces.
