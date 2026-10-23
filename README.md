# Expense Tracker - CRUD with JDBC and MySQL

A simple console program: one Java file, one SQL script, no frameworks, no frontend.

## Files
```
ExpenseTracker/
├── setup.sql           creates the database, table and 4 sample expenses
├── ExpenseTracker.java the whole program
└── README.md
```

## Setup (Windows)
1. Create the database:  `mysql -u root -p < setup.sql`
2. Put your MySQL password in `ExpenseTracker.java` (the `PASSWORD` line).
3. Get the MySQL driver into this folder:
   `curl -L -O https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/8.4.0/mysql-connector-j-8.4.0.jar`
4. Compile:  `javac ExpenseTracker.java`
5. Run:      `java -cp ".;mysql-connector-j-8.4.0.jar" ExpenseTracker`

(macOS/Linux: use `:` instead of `;` in the -cp value.)

## CRUD to SQL
| Operation | Menu option | SQL | JDBC method |
|---|---|---|---|
| Create | 1 Add | INSERT INTO expenses ... | executeUpdate() |
| Read | 2 View all, 3 Search | SELECT ... | executeQuery() |
| Update | 4 Update | UPDATE expenses SET ... WHERE id = ? | executeUpdate() |
| Delete | 5 Delete | DELETE FROM expenses WHERE id = ? | executeUpdate() |
| Bonus | 6 Report | SELECT category, SUM(amount) ... GROUP BY category | executeQuery() |

## Sample output
```
===== EXPENSE TRACKER =====
Choose an option: 2

--- All Expenses ---
ID   Title                  Category            Amount  Date
----------------------------------------------------------------------
4    Groceries              Food               1200.50  2026-10-23
3    Movie tickets          Entertainment       600.00  2026-10-22
2    Metro card recharge    Transport           500.00  2026-10-21
1    Lunch at cafe          Food                250.00  2026-10-20

Choose an option: 6

--- Spending by Category ---
Category           Items   Total
--------------------------------------
Food               2        1450.50
Entertainment      1         600.00
Transport          1         500.00
--------------------------------------
GRAND TOTAL                2550.50
```

## How it works
- **JDBC steps:** open a `Connection`, create a `PreparedStatement` with `?` placeholders, fill them with `setString` / `setBigDecimal` / `setDate`, run it, read rows with `while (rs.next())`.
- **PreparedStatement** keeps user input separate from the SQL, which prevents SQL injection.
- **try-with-resources** closes the connection, statements and result sets automatically.
- **BigDecimal** is used for money because `double` can give rounding errors.
- **Validation:** helper methods keep asking until the input is valid (positive amount, correct date format, no empty fields). The database also has a CHECK (amount > 0).
- **Error handling:** any database error prints a message and the menu continues.

## Troubleshooting
| Problem | Fix |
|---|---|
| Access denied for user 'root' | Wrong PASSWORD in ExpenseTracker.java |
| Unknown database 'expense_db' | Run setup.sql |
| No suitable driver found | Driver jar missing from the folder or the -cp value is wrong |
| Communications link failure | MySQL is not running |
"# java-expense-tracker" 
