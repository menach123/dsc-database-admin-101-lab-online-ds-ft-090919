
# Database Admin 101 - Lab

## Introduction 

In this lab, you'll go through the process of designing and creating a database. From there, you'll begin to populate this table with mock data provided to you.

## Objectives

You will be able to:

* Create a SQL database
* Create a SQL table
* Create rows in a SQL table
* Alter entries in a SQL table
* Delete entries in a SQL table
* Commit changes via sqlite3


## The Scenario

You are looking to design a database for a school that will house various information from student grades to contact information, class roster lists and attendance. First, think of how you would design such a database. What tables would you include? What columns would each table have? What would be the primary means to join said tables?

## Creating the Database

Now that you've put a little thought into how you might design your database, it's time to go ahead and create it! Start by import the necessary packages. Then, create a database called **school.sqlite**.


```python
import sqlite3 
import pandas as pd

```


```python
#Your code here; create the database school.sqlite
conn = sqlite3.connect('school.db')
cur = conn.cursor()
```

##### Function


```python
def dict_convert(dict_id_format):
    list_return = []
    for i in dict_id_format:
        list_return.append(i)
        return list_return
    
```

## Create a Table for Contact Information

Create a table called contactInfo to house contact information for both students and staff. Be sure to include columns for first name, last name, role (student/staff), telephone number, street, city, state, and zipcode. Be sure to also create a primary key for the table. 


```python
cur.execute("""CREATE TABLE contactInfo (id INTEGER PRIMARY KEY,
                                firstName TEXT, 
                                lastName TEXT,
                                role TEXT, 
                                telephone INTEGER, 
                                street TEXT, 
                                city TEXT, state TEXT,
                                zipcode INTEGER)
            """)

```




    <sqlite3.Cursor at 0x18526346570>



## Populate the Table

Below, code is provided for you in order to load a list of dictionaries. Briefly examine the list. Each dictionary in the list will serve as an entry for your contact info table. Once you've briefly investigated the structure of this data, write a for loop to iterate through the list and create an entry in your table for each person's contact info.


```python
# Code to load the list of dictionaries; just run this cell
import pickle

with open('contact_list.pickle', 'rb') as f:
    contacts = pickle.load(f)

```


```python
# Your code to iterate over the contact list and populate the contactInfo table here
for contact in contacts:
    firstName = contact['firstName']
    lastName = contact['lastName']
    role = contact['role']
    telephone  = contact['telephone ']
    street = contact['street']
    city = contact['city']
    state = contact['state']
    zipcode  = contact['zipcode ']
    cur.execute("""INSERT INTO contactInfo (firstName, lastName, role, telephone, street, city, state, zipcode) 
                  VALUES ('{}', '{}', '{}', '{}', '{}', '{}', '{}', '{}');
                """.format(firstName, lastName, role, telephone, street, city, state, zipcode) )
```

**Query the Table to Ensure it is populated**


```python
cur.execute("""
SELECT * 
FROM contactInfo""")
df = pd.DataFrame(cur.fetchall(), columns=[x[0] for x in cur.description])
df = df.set_index('id')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>role</th>
      <th>telephone</th>
      <th>street</th>
      <th>city</th>
      <th>state</th>
      <th>zipcode</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Christine</td>
      <td>Holden</td>
      <td>staff</td>
      <td>2035687697</td>
      <td>1672 Whitman Court</td>
      <td>Stamford</td>
      <td>CT</td>
      <td>6995</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Christopher</td>
      <td>Warren</td>
      <td>student</td>
      <td>2175150957</td>
      <td>1935 University Hill Road</td>
      <td>Champaign</td>
      <td>IL</td>
      <td>61938</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Linda</td>
      <td>Jacobson</td>
      <td>staff</td>
      <td>4049446441</td>
      <td>479 Musgrave Street</td>
      <td>Atlanta</td>
      <td>GA</td>
      <td>30303</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andrew</td>
      <td>Stepp</td>
      <td>student</td>
      <td>7866419252</td>
      <td>2981 Lamberts Branch Road</td>
      <td>Hialeah</td>
      <td>Fl</td>
      <td>33012</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Jane</td>
      <td>Evans</td>
      <td>student</td>
      <td>3259909290</td>
      <td>1461 Briarhill Lane</td>
      <td>Abilene</td>
      <td>TX</td>
      <td>79602</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Mary</td>
      <td>Raines</td>
      <td>student</td>
      <td>9075772295</td>
      <td>3975 Jerry Toth Drive</td>
      <td>Ninilchik</td>
      <td>AK</td>
      <td>99639</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Ed</td>
      <td>Lyman</td>
      <td>student</td>
      <td>5179695576</td>
      <td>3478 Be Sreet</td>
      <td>Lansing</td>
      <td>MI</td>
      <td>48933</td>
    </tr>
  </tbody>
</table>
</div>



## Commit Your Changes to the Database

Persist your changes by committing them to the database.


```python
conn.commit()
```

## Create a Table for Student Grades

Create a new table in the database called "grades". In the table, include the following fields: userId, courseId, grade.

** This problem is a bit more tricky and will require a dual key. (A nuance you have yet to see.)
Here's how to do that:

```SQL
CREATE TABLE table_name(
   column_1 INTEGER NOT NULL,
   column_2 INTEGER NOT NULL,
   ...
   PRIMARY KEY(column_1,column_2,...)
);
```


```python
cur.execute("""CREATE TABLE grades (userId INTEGER NOT NULL, 
                                    courseId INTEGER NOT NULL, 
                                    grade TEXT,
                                    PRIMARY KEY(userId, courseId));""")
```




    <sqlite3.Cursor at 0x18526346570>



## Remove Duplicate Entries

An analyst just realized that there is a duplicate entry in the contactInfo table! Find and remove it.


```python
cur.execute("""SELECT firstName, lastName, telephone, COUNT(*) 
               FROM contactInfo
               GROUP BY 1,2,3
               HAVING COUNT(*) > 1;""")
cur.fetchall()
```




    [('Jane', 'Evans', 3259909290, 2)]




```python
cur.execute('''DELETE FROM contactInfo WHERE telephone = 3259909290;''')

```




    <sqlite3.Cursor at 0x18526346570>




```python
cur.execute("""SELECT firstName, lastName, telephone, COUNT(*) 
               FROM contactInfo
               GROUP BY 1,2,3
               HAVING telephone == 3259909290;""")
df = pd.DataFrame(cur.fetchall(), columns=[x[0] for x in cur.description])

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>telephone</th>
      <th>COUNT(*)</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



## Updating an Address

Ed Lyman just moved to `2910 Simpson Avenue York, PA 17403`. Update his address accordingly.


```python
cur.execute('''UPDATE contactInfo
                  SET street = '2910 Simpson Avenue', city = 'York', state = 'PA'
                  WHERE lastName = 'Lyman' and firstName = 'Ed'
            ''')
```




    <sqlite3.Cursor at 0x18526346570>




```python
cur.execute("""SELECT firstName, lastName, street, city, 
               FROM contactInfo
               WHERE lastName = 'Lyman' and firstName = 'Ed'
               ;""")
df = pd.DataFrame(cur.fetchall(), columns=[x[0] for x in cur.description])

df
```

## Commit Your Changes to the Database

Once again, persist your changes by committing them to the database.


```python
#Your code here
```

## Summary

While there's certainly more to do with setting up and managing this database, you got a taste for creating, populating, and maintaining databases! Feel free to continue fleshing out this exercise for more practice. 
