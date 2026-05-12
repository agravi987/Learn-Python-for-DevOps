# 15-Database-Basics 🗄️

Databases are useful in DevOps automation when you need to store metrics, inventory, job history, audit events, deployment records, or script results. Python can work with lightweight local databases like SQLite and production databases like PostgreSQL.

## 🎯 What You Will Learn

- When DevOps scripts need a database.
- How to use SQLite with Python.
- How SQL basics work.
- How SQLAlchemy helps structure database code.
- How to build a mini deployment history tracker.

## 🧠 Why Databases Matter in DevOps

Flat files are fine for simple reports, but databases help when data grows.

Use a database for:

- Deployment history.
- Server inventory.
- Monitoring samples.
- Incident records.
- Backup job status.
- CI/CD run metadata.

## 🧰 Common Libraries

- `sqlite3`: built into Python, good for local scripts and small tools.
- `psycopg2`: PostgreSQL driver.
- `asyncpg`: async PostgreSQL driver.
- `sqlalchemy`: ORM and SQL toolkit.

Install external packages:

```bash
pip install sqlalchemy psycopg2-binary
```

`sqlite3` does not need installation.

## 📦 SQLite Basics

SQLite stores the database in one file.

```python
import sqlite3

connection = sqlite3.connect("devops.db")
cursor = connection.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS deployments (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    service TEXT NOT NULL,
    environment TEXT NOT NULL,
    version TEXT NOT NULL,
    status TEXT NOT NULL
)
""")

connection.commit()
connection.close()
```

## ➕ Insert Data

Use parameterized queries to avoid SQL injection.

```python
import sqlite3

connection = sqlite3.connect("devops.db")
cursor = connection.cursor()

cursor.execute(
    "INSERT INTO deployments (service, environment, version, status) VALUES (?, ?, ?, ?)",
    ("payment-api", "prod", "1.2.0", "success")
)

connection.commit()
connection.close()
```

Avoid building SQL with f-strings when values come from users.

## 🔍 Query Data

```python
import sqlite3

connection = sqlite3.connect("devops.db")
cursor = connection.cursor()

cursor.execute("SELECT service, environment, version, status FROM deployments")
rows = cursor.fetchall()

for row in rows:
    print(row)

connection.close()
```

You can also get dictionary-like rows:

```python
connection.row_factory = sqlite3.Row
```

## 🔄 Update and Delete

Update:

```python
cursor.execute(
    "UPDATE deployments SET status = ? WHERE id = ?",
    ("failed", 1)
)
```

Delete:

```python
cursor.execute("DELETE FROM deployments WHERE id = ?", (1,))
```

Always be careful with `DELETE` and `UPDATE`. Add `WHERE` clauses intentionally.

## 🧱 SQLAlchemy Basics

SQLAlchemy can make database code more structured.

```python
from sqlalchemy import create_engine, text

engine = create_engine("sqlite:///devops.db")

with engine.connect() as connection:
    result = connection.execute(text("SELECT 1"))
    print(result.scalar())
```

SQLAlchemy is useful when projects grow or need to support different databases.

## 🛡️ Database Safety Tips

- Use parameterized queries.
- Close database connections.
- Commit changes intentionally.
- Back up important databases.
- Avoid storing plaintext secrets.
- Keep migrations under version control for real applications.

## 🛠️ Mini Project: Deployment History Tracker

Goal: Build a small SQLite database that stores deployment records and prints a report.

### Step 1: Create the Database Table

```python
import sqlite3

connection = sqlite3.connect("deployments.db")
cursor = connection.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS deployments (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    service TEXT NOT NULL,
    environment TEXT NOT NULL,
    version TEXT NOT NULL,
    status TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
""")

connection.commit()
```

### Step 2: Insert a Deployment

```python
cursor.execute(
    """
    INSERT INTO deployments (service, environment, version, status)
    VALUES (?, ?, ?, ?)
    """,
    ("payment-api", "prod", "1.2.0", "success")
)

connection.commit()
```

### Step 3: Query Deployments

```python
cursor.execute("""
SELECT service, environment, version, status, created_at
FROM deployments
ORDER BY created_at DESC
""")

rows = cursor.fetchall()
```

### Step 4: Print a Report

```python
for service, environment, version, status, created_at in rows:
    icon = "✅" if status == "success" else "🚨"
    print(f"{icon} {service} {version} to {environment} - {status} at {created_at}")
```

### Step 5: Close the Connection

```python
connection.close()
```

### Step 6: Improve It

- Add a CLI with `add` and `list` commands.
- Store rollback version.
- Filter by environment.
- Export deployment history to JSON.
- Add tests for insert and query functions.

## ✅ Quick Revision

- Databases store structured automation data.
- SQLite is great for learning and local tools.
- Use parameterized queries for safety.
- SQLAlchemy helps when projects grow.
- DevOps scripts can store metrics, inventory, and deployment history.
