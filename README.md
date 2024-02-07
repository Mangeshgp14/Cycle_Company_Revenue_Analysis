<h1 align='center'>🚲 Cycle Manufacturing Company Revenue Analysis 🚲</h1>

![AdventureWorks_Logo](https://github.com/Mangeshgp14/Cycle_Company_Revenue_Analysis/assets/107695842/cc4290c0-5cf8-4554-9c51-a037e3c7a546)


**Situation 🧑‍💼:**
- You have been hired as a Data Analyst by AdventureWorks, a global manufacturing company that produces cycling equipment and accessories.

**Project Brief 📄:**
\
The Management team needs a way to
- Track KPIs (sales, revenue, profit, returns)
- Compare regional performance
- Analyze product-level trends
- Identify high-value customers.

**Input Data 📁:**
- The organization has provided a folder of raw CSV files that contain information about transactions, returns, products, and sales territories.

**Tech Stack 👩‍💻:**
- Excel and PowerBI

**Workflow ⏩:**
- Connect and Transform the raw data
- Build a relational data model
- Create calculated columns and measures with DAX
- Design an interactive dashboard to visualize the data

**1. Building the Data Pipeline ⚙️:**
- The data files provided by the client are in the form of CSV files.
- We will use Python to load these files into MySQL Database.
- MySQL Database will be connected to Power BI as the main data source.

**1.1. Python Script to load the dataset(CSV files) into MySQL database**

````PYTHON
# Installing necessary libraries
#!pip install mysql-connector-python
#!pip install pandas
#!pip install sqlalchemy
````

````PYTHON
# Importing Libraries
import pandas as pd
from sqlalchemy import create_engine
import config
````

````PYTHON
# MySQL database configuration
db_config = {
    'user': config.username,
    'password': config.password,
    'host': config.host,
    'raise_on_warnings': True
}

# Create a SQLAlchemy engine to connect to MySQL server
mysql_engine = create_engine(f"mysql+mysqlconnector://{db_config['user']}:{db_config['password']}@{db_config['host']}")
````
````PYTHON
# Create a new database
new_database_name = 'adventure_works_database'
with mysql_engine.connect() as connection:
    connection.execute(f"CREATE DATABASE IF NOT EXISTS {new_database_name}")

# Connect to the newly created database
db_config['database'] = new_database_name
engine = create_engine(f"mysql+mysqlconnector://{db_config['user']}:{db_config['password']}@{db_config['host']}/{db_config['database']}")

````


````PYTHON
# Loading CSV files as tables into the database

csv_files = ['AdventureWorks Calendar Lookup', 'AdventureWorks Customer Lookup', 'AdventureWorks Product Categories Lookup', 'AdventureWorks Product Lookup', 'AdventureWorks Product Subcategories Lookup', 'AdventureWorks Returns Data', 'AdventureWorks Sales Data 2020', 'AdventureWorks Sales Data 2021', 'AdventureWorks Sales Data 2022', 'AdventureWorks Territory Lookup', 'Product Category Sales (Unpivot Demo)']

for i in csv_files :
    
    # CSV file path
    csv_file_path = f'{i}.csv'

    # Table name in MySQL
    table_name = f'{i}'

    # Read CSV into a Pandas DataFrame
    df = pd.read_csv(csv_file_path)

    # Insert DataFrame records into MySQL using SQLAlchemy
    df.to_sql(name=table_name, con=engine, if_exists='replace', index=False)

    print("Data imported successfully.")
````
