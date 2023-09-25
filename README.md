# sqlite_database_operations

In this assignment, I integrated data processing with Python, used Pandas for exploratory data analysis, and conducted database operations using SQLite.

### Datasets

The two datasets I utilized for this assignment are from New York Presbyterian Hospital and Stony Brook University Hospital. Each dataset contains data relating to descriptions of hospital services, descriptions of codes, billing codes, billing charges, gross charges, and insurance data. 

### Data Exploration and Analysis 

For this portion of the assignment, I focused on basic exploratory data analysis. Using Python, I examined the size and shape of columns, column names, cleaning column names, and checking for missing values. For missing values, I used the drop function to remove them from the dataset for a more accurate visualization of the data. I also performed basic descriptive statistics to examine frequency counts (inpatient/outpatient) as well as finding the mean, median, and mode of certain numerical columns (gross charges). I also created histograms and a scatter plot to further visualize the numerical data. 

### SQLite Database Operations 

Here are the steps to replicate the SQLite database setup:

1. Load in the following packages: ```from sqlalchemy import create_engine``` and ```import sqlite3``` If you find that SQLAlchemy is not installed, you can use the command ```pip install sqlalchemy```
2. Create a temporary and local database called "health.db" using:
conn = sqlite3.connect('health.db')
c = conn.cursor()
3. Manually create a table titled "health_data" which will contain at least 5 columns. At least two columns should be numerical columns and at least two columns should be categorical (string) columns. I used patient name, diagnosis, age, weight (lbs), height (in), insurance, and gender.
c.execute('''
            CREATE TABLE health_data (
              patient_name TEXT,
              diagnosis TEXT,
              age INTEGER,
              weight_lbs REAL,
              height_in REAL,
              insurance TEXT,
              gender TEXT
            );
        ''')
conn.commit()
4. Enter the following command to ensure that the table "health_data" has been created within in the health.db database
c.execute('''
  SELECT name
  FROM sqlite_master
  WHERE type='table';
  ''')

c.fetchall()
6. Now we are going to use the INSERT INTO SQL query command to populate the health_data table we created with fake data:
sql_query = '''
INSERT INTO health_data
(
  'patient_name',
  'diagnosis',
  'age',
  'weight_lbs',
  'height_in',
  'insurance',
  'gender'
)
values
('Sarah', 'diabetes', 45, 150.2, 67, 'medicare', 'F'),
('Mark', 'hypertension', 39, 190.8, 74, 'emblem', 'M'),
('Eliza', 'insomnia', 31, 163, 68, 'medicaid', 'F');
'''
7. Execute the query 
c.execute(sql_query)
conn.commit()
8. To check that the data has been inserted into the table, use PANDAS. This will display the data in a dataframe
pd.read_sql_query("select * from health_data;", conn)
9. For automatic table creation, I used the dataset from Stony Brook University Hospital and utilized the ```to_sql``` function to enter the data into the health.db database
df_stonybrook.to_sql('health_data', conn, if_exists='replace')
10. Run a query to see that the table has been created
query = """
  select *
  from health_data
  where type = 'Inpatient'
  limit 100;
"""

response = pd.read_sql(query, conn)
response



