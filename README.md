# DURHAM COLLEGE
## Data Analytics for Business Decision Making
### Data1202-Data Tools and Analytics - Assignment5

The document is created to guide users on testing a python code do solve the questions. 
- Question 1: Was the average of global sales higher before or after 2005?
- Question 2: Create a new column that labels records before 2005 as 'pre-2005' and after 2005 as 'post-2005'

By using the dataset "vgsales.csv" under folder [Data1202/Dataset/](https://github.com/JecinthaThekkodan/Data1202/tree/main/Dataset)

This documents includes:
- How to install Jupyter - Python
- How to install MySQL
- Overview on the tests conducted
- Run tests based on the requirement/questions


#### Installation

- Use the link [Jupyter installation](https://test-jupyter.readthedocs.io/en/latest/install.html) to install Jupyter.

```bash
pip3 install jupyter
```

- Use the link to install MySQL [MySql Installation](https://www.javatpoint.com/how-to-install-mysql)

#### Prerequisites
- Jupyter should be installed
- MySQL should be installed
- Dataset to be downloaded under folder [Data1202/Dataset/](https://github.com/JecinthaThekkodan/Data1202/tree/main/Dataset)
- Download the python file under folder [Data1202/](https://github.com/JecinthaThekkodan/Data1202/tree/main)
- Open the python file notebook in Jupyter
- Connect to MYSQL
- Load the data set in MYSQL in vgsales table

#### Running the Tests

```python
#Import Python Libraries
import mysql.connector as sql
import pandas as pd

#Build the connection to database
# Replace XXXX with username
# Replace YYYY with password
# Replace ZZZZ with DB schema
conn=sql.connect(host='localhost',user='XXXX',password='YYYY',db='ZZZZ')
conn

# Read a query into DataFrame
# The SELECT statement will read all the rows from vgsales table into the dataframe df
df=pd.read_sql_query("SELECT * FROM data1202.vgsales", conn)
df

# Question 1: Was the average of global sales higher before or after 2005?

# Using PANDAS to read SQL query that fetches 
# - sales average before 2005
# - sales average after 2005
# Case statement is used to compare and calculate which sales average is higher
# AVG, ROUND are aggregate functions in SQL to calculate the mean and round upto 2 digits respectively
qs1_df = pd.read_sql('''Select a.before2005 as Sales_Avg_Before_2005, b.after2005 as Sales_Avg_After_2005,
    (case
    when a.before2005 > b.after2005 then 'Global sales were higher before 2005'
    else 'Global sales were higher after 2005'
    end) as Result
From
    (select ROUND(AVG(global_sales),2) as before2005 from vgsales where year < 2005) a,
    (select ROUND(AVG(global_sales),2) as after2005 from vgsales where year > 2005) b''', conn)

#Using below cmnd to print result horizontally in 1 row
with pd.option_context('expand_frame_repr', False):
    print(qs1_df)

# Question 2: Create a new column that labels records before 2005 as 'pre-2005' and after 2005 as 'post-2005'
# Using PANDAS to read SQL query which creates a new column 
# pre-2005
# post-2005
# in-2005
# based on the case statement fulfilled
# Case statement is used to compare and calculate which sales average is higher
# AVG, ROUND are aggregate functions in SQL to calculate the mean and round upto 2 digits respectively

qs2_df = pd.read_sql('''Select
    (case
    when year < 2005 then 'pre-2005'
    when year > 2005 then 'post-2005'
    else  'in-2005'
    end) as Indicator, vgsales.*
    from vgsales;''', conn)

# Displaying the newly added column - Indicator
qs2_df

```

#### Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

#### License
This project is licensed under Durham College ®
