# sqlite fun
Doing some playing today with sqlite3 module for an upcoming project idea.

This link was very helpful, https://www.pythoncentral.io/introduction-to-sqlite-in-python/

Connecting to my database
1. Importing sqlite3
2. Connecting db=sqlite3.connect('test.sqllite')
3. Create a cursor, which seems like a shim between the db connection object and calls to the database, cursor=db.cursor()

4. Perform a select on my database, cursor.execute('''SELECT name, email, phone from users''')

5. Viewing results of one row, user1=cursor.fetchone()

print(user1[0])
Freddy Kruger

```
>>> import sqlite3
>>> db=sqlite3.connect('test.sqllite')
>>> cursor=db.cursor()
>>> cursor.execute('''CREATE TABLE users(id INTEGER PRIMARY KEY, name TEXT, phone TEXT, email TEXT unique, password TEXT)
... ''')
<sqlite3.Cursor object at 0x7fc9172e2d50>
>>> db.commit()
>>> cursor.execute('''SELECT name, email, phone from users''')
<sqlite3.Cursor object at 0x7fc9172e2d50>
>>> user1=cursor.fetchone()
>>> print(user1[0])
Freddy Kruger
>>> print(user1[1])
fkruger@aol.com

```

To save time I repopulated my database with sqlitebrowser UI

Here's what my table looks like, using sqlite3 cli tool:

```
SQLite version 3.11.0 2016-02-15 17:29:24
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite> .open test.sqllite
sqlite> .tables
users
sqlite> .schema users
CREATE TABLE users(id INTEGER PRIMARY KEY, name TEXT, phone TEXT, email TEXT unique, password TEXT);
sqlite> select * from users;
1|Freddy Kruger|312-700-1234|fkruger@aol.com|123456
2|Jason Borne|312-606-9123|dangerous1@yahoo.com|bottomsup1
3|Fred Flintstone|847-438-0002|stone@rr.br.co|neverneeded1
```

More to come. my time today is very limited today. The project I'm planning should be 
fun and take advantage of storing data in a  sqlite database for long term review.   
