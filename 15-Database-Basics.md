# 15-Database-Basics

## Libraries

- sqlite3: Built-in for SQLite
- psycopg2: `pip install psycopg2` for PostgreSQL
- sqlalchemy: `pip install sqlalchemy` ORM

## sqlite3 Usage

- Connect: `conn = sqlite3.connect("db.sqlite")`
- Cursor: `c = conn.cursor()`
- Execute: `c.execute("SELECT * FROM table")`
- Commit: `conn.commit()`

## SQLAlchemy

- Engine: `engine = create_engine("sqlite:///db.sqlite")`
- Session: `Session = sessionmaker(bind=engine)`
- Query: `session.query(Model).filter(...)`

## Practice

- Log monitoring data to SQLite
- Query metrics from PostgreSQL in scripts
