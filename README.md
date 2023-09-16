import tkinter as tk
import mysql.connector

# Connect to MySQL (update these values with your database information)
db = mysql.connector.connect(
    host="your_host",
    user="your_username",
    password="your_password",
    database="hospital"
)

cursor = db.cursor()

def save_appointment(name, department):
    try:
        # SQL query to insert appointment data into the appointments table
        insert_query = "INSERT INTO appointments (name, department) VALUES (%s, %s)"
        values = (name, department)

        cursor.execute(insert_query, values)
        db.commit()

        print(f"Appointment booked for {name} in the {department} department.")
    except mysql.connector.Error as err:
        print(f"Error: {err}")

def book_appointment():
    book_window = tk.Toplevel(root)
    book_window.title("Book Appointment")
    book_window.configure(bg="lightblue")

    name_label = tk.Label(book_window, text="Name:")
    name_label.pack()

    name_entry = tk.Entry(book_window)
    name_entry.pack()

    department_label = tk.Label(book_window, text="Department:")
    department_label.pack()

    department_entry = tk.Entry(book_window)
    department_entry.pack()

    book_button = tk.Button(book_window, text="Book", command=lambda: save_appointment(name_entry.get(), department_entry.get()))
    book_button.pack()

def cancel_appointment():
    cancel_window = tk.Toplevel(root)
    cancel_window.title("Cancel Appointment")
    cancel_window.configure(bg="lightblue")

    cancel_label = tk.Label(cancel_window, text="Cancel your appointment here.")
    cancel_label.pack()

def retrieve_appointments():
    cursor.execute("SELECT name, department FROM appointments")
    appointments = cursor.fetchall()
    return appointments

root = tk.Tk()
root.title("Hospital Management System")
root.configure(bg="lightblue")

book_button = tk.Button(root, text="Book", command=book_appointment)
book_button.pack()

cancel_button = tk.Button(root, text="Cancel", command=cancel_appointment)
cancel_button.pack()

appointments_button = tk.Button(root, text="View Appointments", command=lambda: display_appointments(retrieve_appointments()))
appointments_button.pack()

def display_appointments(appointments):
    appointments_window = tk.Toplevel(root)
    appointments_window.title("Appointments")
    appointments_window.configure(bg="lightblue")

    if appointments:
        for i, (name, department) in enumerate(appointments, start=1):
            label = tk.Label(appointments_window, text=f"Appointment {i}: {name} in {department} department")
            label.pack()
    else:
        no_appointments_label = tk.Label(appointments_window, text="No appointments found.")
        no_appointments_label.pack()

root.mainloop()


<!---
minx-3/minx-3 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
