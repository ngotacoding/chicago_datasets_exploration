# DATABASES AND SQL

## Project Summary

Three datasets, [The Chicago Census Data dataset](<https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCensusData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01>), [Chicago Public Schools dataset](<https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01>), and [The Chicago Crime Data dataset](<https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCrimeData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01>), are loaded into individual SQL tables in an sqlite3 database, and explored using [Ipython-sql](https://pypi.org/project/ipython-sql/) extension and the [sqlite3](https://docs.python.org/3/library/sqlite3.html) python library.

## Libraries and Tools used

The project uses [pandas](https://pandas.pydata.org/pandas-docs/stable/reference/index.html), [sqlite3](https://docs.python.org/3/library/sqlite3.html), [csv](https://docs.python.org/3/library/csv.html), and [warnings](https://docs.python.org/3/library/warnings.html) libraries. [Pandas](https://pandas.pydata.org/pandas-docs/stable/reference/index.html) is used to create and manipulate dataframes from csv files, [sqlite3](https://docs.python.org/3/library/sqlite3.html) to interact with the database using python, and [warnings](https://docs.python.org/3/library/warnings.html) to filter warning messages.
[Ipython-sql](https://pypi.org/project/ipython-sql/) and [sqlalchemy](https://www.sqlalchemy.org/) are used to enable sql magic methods in the jupyter-notebook.

### Installing the libraries

[Sqlite3](https://docs.python.org/3/library/sqlite3.html), [csv](https://docs.python.org/3/library/csv.html), and [warnings](https://docs.python.org/3/library/warnings.html) come in the Python Standard Library.  
[Pandas](https://pandas.pydata.org/pandas-docs/stable/reference/index.html), [ipython-sql](https://pypi.org/project/ipython-sql/), and [sqlalchemy](https://www.sqlalchemy.org/) are installed using the `pip install` command.

```bash
pip install pandas
pip install sqlalchemy
pip install ipython-sql
```

Make sure to initialize the ipython-sql extension to enable use of sql magic methods by running `%load_ext sql`, then import the libraries used.  
Learn more on Python Standard Library [Here](https://docs.python.org/3/library/).

## The Process

- The process starts with these lines of code:

  ```python
  con = sqlite3.connect("./SQLiteDB.db")
  cur = con.cursor()
  ```
  
  - where:

    - `con = sqlite3.connect("SQLiteDB.db")` creates an [sqlite3 connection object](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection) to the database in which our tables will be stored. The connection object is assigned to the variable `con`. The argument is the path to the database.  
    - `cur = con.cursor()` creates a [cursor object](https://docs.python.org/3/library/sqlite3.html#sqlite3.Cursor) for operations on our database using the established connection object, `con`.  

- Run `%sql sqlite:///SQLiteDB.db` to create a connection for the ipython-sql extension to the database.  

- Use `pd.read_csv(<url_to_dataset>)` to load individual datasets into unique [pandas dataframes](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html). Ensure to replace _<url_to_dataset>_ with the real url to the dataset.  
- Each dataframe is then made into an sql table by running the following line of code:

  ```python
  dataframe.to_sql(name_of_sql_table, con, if_exists= 'replace', index=False, method= 'multi')
  ```

  - where:

    - `dataframe` is the individual dataframe.
    - `name_of_sql_table` is the designated name for the resultant table in the database.
    - `con` is the connection to database.
    - `if_exists= 'replace'` checks for the existence of the table in the database, and if found, drops the old table and replaces with the current table.
    - `index=False` specifies that the row labels should not be written as a column in the sql table.
    - `method = 'multi'` allows to pass multiple values in a single insert clause.  

  - For more on the method `pd.DataFrame.to_sql()` check the documentation [here](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_sql.html)

- We can now write queries to the database using sql magic by using the prefix `%sql` before each single line query, or `%%sql` before each multi-line query.

  Example:

  ```python
  %sql SELECT * FROM CENSUS_DATA LIMIT 5;
  ```

  or

  ```python
  %%sql
  SELECT * 
  FROM CENSUS_DATA 
  LIMIT 5;
  ```

## About

Final project submitted for completion of the [Databases and SQL for Data Science with Python](https://www.coursera.org/learn/sql-data-science) Coursera course by IBM.
