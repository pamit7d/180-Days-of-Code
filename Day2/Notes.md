## 📒 Notebook Learning Notes

### 1. Working with JSON Data

#### 🔑 Kaggle API Setup

* Learned how to configure Kaggle API credentials using `kaggle.json`.
* Stored the API key in `~/.kaggle/` for programmatic dataset access.

#### 🔍 Listing Datasets

* Used the following command to search for datasets by keyword:

```bash
kaggle datasets list -s "search_term"
```

#### 📥 Downloading Datasets

* Downloaded datasets directly from Kaggle:

```bash
kaggle datasets download -d dataset_name
```

#### 📦 Extracting ZIP Files

* Used Python's built-in `zipfile` module to extract downloaded datasets.

#### 📄 Reading Local JSON Files

* Loaded JSON data into a Pandas DataFrame using:

```python
pd.read_json("file.json")
```

#### 🌐 Reading JSON Data from APIs

* Retrieved JSON data directly from a web API and converted it into a DataFrame:

```python
pd.read_json("https://restcountries.com/v3.1/all")
```

#### 📊 Reading CSV Files from URLs

* Loaded CSV data directly from a URL:

```python
pd.read_csv("https://example.com/data.csv")
```

#### 🚀 Advanced API Interaction

Worked with external APIs using the `http.client` module:

* Established HTTP connections.
* Configured request payloads and headers.
* Sent API requests and received responses.
* Decoded byte responses into UTF-8 strings.
* Parsed JSON responses using:

```python
json.loads(response_data)
```

* Accessed nested JSON structures:

```python
response["returnvalue"]["data"]
```

* Converted API responses directly into Pandas DataFrames:

```python
df = pd.DataFrame(response["returnvalue"]["data"])
```

---

### 2. Working with SQL Datasets

#### 🗄️ Kaggle SQL Datasets

* Learned that SQL database files can also be downloaded from Kaggle.

#### 💻 Local SQL Environment Setup

* Understood the importance of running a local database server such as:

  * XAMPP
  * MySQL Server
  * MariaDB

* Learned that connecting Google Colab directly to a local SQL server requires additional configuration or tunneling services.

#### 🔌 Connecting to MySQL

* Used `mysql.connector` to establish a connection with a MySQL database.

```python
import mysql.connector

connection = mysql.connector.connect(
    host="localhost",
    user="root",
    password="password",
    database="world"
)
```

#### 📈 Querying SQL Data with Pandas

* Executed SQL queries and loaded results directly into a DataFrame:

```python
pd.read_sql("SELECT * FROM country", connection)
```

* Combined SQL querying capabilities with Pandas for efficient data analysis.

---

### 🎯 Key Takeaways

* Learned how to acquire datasets from Kaggle programmatically.
* Worked with JSON data from local files and web APIs.
* Understood API response handling and JSON parsing.
* Created DataFrames directly from API responses.
* Set up and connected to MySQL databases.
* Queried SQL data and integrated it with Pandas workflows.
* Strengthened foundational skills in data acquisition, preprocessing, and analysis.
