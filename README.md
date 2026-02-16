# DATABASE
DATABASE
import sqlite3
conn = sqlite3.connect("student.db")
cur = conn.cursor()
cur.execute('''
    CREATE TABLE IF NOT EXISTS student (
        sid TEXT PRIMARY KEY,
        sname TEXT,
        s1 INTEGER,
        s2 INTEGER,
        s3 INTEGER,
        total INTEGER,
        avg REAL
    )
''')
conn.commit()

def insert_marks():
    sid = input("Enter Student ID: ")
    sname = input("Enter Student Name: ")
    s1 = int(input("Enter Marks for Subject 1: "))
    s2 = int(input("Enter Marks for Subject 2: "))
    s3 = int(input("Enter Marks for Subject 3: "))
    total = s1 + s2 + s3
    avg = total / 3

    try:
        cur.execute("INSERT INTO student VALUES (?, ?, ?, ?, ?, ?, ?)",
                    (sid, sname, s1, s2, s3, total, avg))
        conn.commit()
        print("Marks Inserted Successfully.\n")
    except sqlite3.IntegrityError:
        print("Student ID already exists.\n")

def update_marks():
    sid = input("Enter Student ID to Update: ")
    cur.execute("SELECT * FROM student WHERE sid = ?", (sid,))
    row = cur.fetchone()

    if row:
        s1 = int(input("Enter New Marks for Subject 1: "))
        s2 = int(input("Enter New Marks for Subject 2: "))
        s3 = int(input("Enter New Marks for Subject 3: "))
        total = s1 + s2 + s3
        avg = total / 3

        cur.execute('''
            UPDATE student SET s1=?, s2=?, s3=?, total=?, avg=?
            WHERE sid=?
        ''', (s1, s2, s3, total, avg, sid))
        conn.commit()
        print("Marks Updated Successfully.\n")
    else:
        print("Invalid Hallticket Number\n")

def display_marks():
    sid = input("Enter Student ID to Display: ")
    cur.execute("SELECT * FROM student WHERE sid = ?", (sid,))
    row = cur.fetchone()

    if row:
        print("\nStudent Name: {row[1]}")
        print("Marks: S1={row[2]}, S2={row[3]}, S3={row[4]}")
        print("Total: {row[5]}, Average: {row[6]:.2f}")
        result = "PASS" if row[2] >= 35 and row[3] >= 35 and row[4] >= 35 else "FAIL"
        print("Result: {result}\n")
    else:
        print("Invalid Hallticket Number\n")

# Menu Loop
while True:
    print("\n--- Student Marks Menu ---")
    print("1. Insert Marks")
    print("2. Update Marks")
    print("3. Display Marks")
    print("4. Exit")

    choice = input("Enter your choice: ")

    if choice == '1':
        insert_marks()
    elif choice == '2':
        update_marks()
    elif choice == '3':
        display_marks()
    elif choice == '4':
        print("Exiting Program.")
        break
    else:
        print("Invalid Choice. Try again.\n")

# Close connection
conn.close()
