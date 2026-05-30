# 🗄️ Day 1: SQL Dataset Handling with Kaggle, XAMPP, MySQL & Pandas

## 🎯 Objective

Learn how to:

* Download SQL datasets from Kaggle
* Set up a local MySQL server using XAMPP
* Import SQL files into MySQL
* Connect Python to MySQL
* Query SQL databases using Pandas
* Understand how local file paths and database connections work

---

# 1️⃣ Downloading SQL Dataset from Kaggle

I downloaded a sample SQL dataset from Kaggle using:

```bash
!kaggle datasets download -d busielmorley/worldcities-pop-lang-rank-sql-create-tbls
```

### What this command does

* Connects to Kaggle using the configured API key.
* Downloads the dataset as a ZIP file.
* Saves it in the current working directory.

The dataset contains SQL scripts that can be imported into a MySQL database.

---

# 2️⃣ Extracting the Dataset

Downloaded datasets are usually compressed.

Example:

```python
import zipfile

with zipfile.ZipFile("worldcities-pop-lang-rank-sql-create-tbls.zip", "r") as zip_ref:
    zip_ref.extractall("dataset")
```

After extraction:

```text
dataset/
├── world.sql
├── README.txt
```

---

# 3️⃣ Saving the SQL File

In my setup, I saved:

```text
world.sql
```

inside the same directory as my notebook.

Example:

```text
Day1/
├── SQL_data_handling.ipynb
├── world.sql
```

Because both files are in the same folder, Python can easily access them using relative paths.

---

## What if the SQL file is somewhere else?

Suppose:

```text
Desktop/
├── SQL_data_handling.ipynb

Downloads/
├── world.sql
```

Then Python cannot find the file automatically.

You must provide the complete path:

```python
sql_file = "/home/amit/Downloads/world.sql"
```

or

```python
sql_file = "C:/Users/Amit/Downloads/world.sql"
```

on Windows.

### Relative Path Example

```python
sql_file = "../Downloads/world.sql"
```

Meaning:

* Go one folder up.
* Enter Downloads.
* Access world.sql.

---

# 4️⃣ Setting Up XAMPP

XAMPP provides:

* Apache
* MySQL/MariaDB
* PHP
* Perl

It helps us create a local database server on our computer.

---

## Starting XAMPP

```bash
sudo /opt/lampp/lampp start
```

---

## Stopping XAMPP

```bash
sudo /opt/lampp/lampp stop
```

---

## Checking Status

```bash
sudo /opt/lampp/lampp status
```

---

# 5️⃣ Accessing phpMyAdmin

After starting XAMPP:

Open:

```text
http://localhost/phpmyadmin
```

### What is localhost?

`localhost` refers to your own computer.

It is equivalent to:

```text
127.0.0.1
```

So:

```text
http://localhost/phpmyadmin
```

means:

> Open phpMyAdmin running on my own machine.

---

## What is phpMyAdmin?

A web interface used to:

* Create databases
* Create tables
* Run SQL queries
* Import SQL files
* Export databases
* Manage users

---

# 6️⃣ Importing the SQL Dataset

After opening phpMyAdmin:

### Step 1

Create a database:

```text
world
```

### Step 2

Click:

```text
Import
```

### Step 3

Select:

```text
world.sql
```

### Step 4

Click:

```text
Go
```

MySQL executes all SQL statements and creates the tables automatically.

---

# 7️⃣ Connecting Python to MySQL

Install connector:

```bash
pip install mysql-connector-python
```

---

## Creating Connection

```python
import mysql.connector

connection = mysql.connector.connect(
    host="localhost",
    user="root",
    password="",
    database="world"
)
```

---

# 8️⃣ Understanding Connection Parameters

## host="localhost"

```python
host="localhost"
```

Tells Python:

> Connect to the MySQL server running on this computer.

If the database were on another machine:

```python
host="192.168.1.10"
```

or

```python
host="example.com"
```

---

## user="root"

```python
user="root"
```

### Why root?

`root` is the default MySQL administrator account.

Think of it like:

```text
Linux Admin Account
```

for MySQL.

Root can:

* Create databases
* Delete databases
* Create users
* Modify tables
* Execute all queries

---

### Should we always use root?

No.

For real projects:

```text
❌ Using root is not recommended
```

Instead:

Create a dedicated user:

```sql
CREATE USER 'amit' IDENTIFIED BY 'mypassword';
GRANT ALL PRIVILEGES ON world.* TO 'amit';
```

Then connect:

```python
user="amit"
password="mypassword"
```

This is safer.

---

## password=""

```python
password=""
```

In many XAMPP installations:

```text
root
```

has no password by default.

For production systems:

```text
Always set a strong password.
```

---

## database="world"

```python
database="world"
```

Tells MySQL:

> Use the database named world after connecting.

Equivalent SQL:

```sql
USE world;
```

---

# 9️⃣ Reading SQL Data into Pandas

Execute query:

```python
import pandas as pd

df = pd.read_sql(
    "SELECT * FROM country",
    connection
)
```

---

### What happens internally?

1. Pandas sends query to MySQL.
2. MySQL executes query.
3. Result is returned.
4. Pandas converts it into a DataFrame.

Result:

```text
+------+---------+------------+
| Code | Name    | Population |
+------+---------+------------+
| IND  | India   | ...        |
| USA  | USA     | ...        |
+------+---------+------------+
```

becomes:

```python
df.head()
```

| Code | Name  | Population |
| ---- | ----- | ---------- |
| IND  | India | ...        |
| USA  | USA   | ...        |

---

# 🔑 Key Learnings

* Downloaded SQL datasets from Kaggle using the Kaggle API.
* Extracted ZIP files using Python.
* Learned the difference between relative and absolute file paths.
* Set up a local MySQL server using XAMPP.
* Imported SQL files using phpMyAdmin.
* Connected Python to MySQL using `mysql.connector`.
* Understood the purpose of `host`, `user`, `password`, and `database`.
* Learned why `root` is used and why custom users are preferred in real applications.
* Queried SQL databases directly into Pandas DataFrames for analysis.
