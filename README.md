import sqlite3

conn = sqlite3.connect('example.db')
cursor = conn.cursor()

cursor.execute('''CREATE TABLE IF NOT EXISTS users
                (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)''')

def insert_user(name, age):
    cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', (name, age))

conn.execute('BEGIN TRANSACTION')

try:
    insert_user('John', 25)
    insert_user('Jane', 30)

    conn.commit()
    print("Transaction committed successfully!")
except Exception as e:
    conn.rollback()
    print("Transaction rolled back:", e)

cursor.execute('SELECT * FROM users')
print("All users:", cursor.fetchall())

conn.close()
