# Vehicle Rental Application

The Vehicle Rental Application is a Python program that allows users to manage vehicle rentals. This project is designed to help you practice your Python programming skills, work with databases, and create a command-line application. It offers the following features:

## Features

1. **Registration & Login**: Users can create an account and log in securely.
2. **Add Vehicle**: Users can add vehicles to the rental inventory, specifying the vehicle type and ID.
3. **List Vehicles**: Users can view the list of available vehicles.
4. **Rent Vehicle**: Users can rent a vehicle, specifying the vehicle ID and rental duration in days.
5. **Return Vehicle**: Users can return a rented vehicle, and the rental cost is calculated based on the rental duration.

## Project Structure

- The program uses an SQLite database to store user information, vehicle data, and rental records.
- User passwords are securely hashed using SHA-512 before being stored in the database.
- The user interacts with the application through a simple command-line interface.

## How to Use

1. Run the Python program.
2. Select from the available options to register, log in, add vehicles, list vehicles, rent a vehicle, return a vehicle, log out, or exit the application.
3. Follow the on-screen prompts to perform your chosen actions.

## Why This Project?

This project serves as a practical exercise for Python programmers to learn about database management, user authentication, and application structure. By completing this project, you will improve your Python skills and gain experience in building command-line applications.

## Suggested Enhancements

To further expand and enhance this project, you can consider adding the following features:

- Updating vehicle information.
- Providing more detailed vehicle descriptions.
- Implementing user-friendly error handling and messages.
- Developing a graphical user interface (GUI) for a more user-friendly experience.

## Code Examples
```python
import hashlib
import sqlite3
from datetime import datetime, timedelta

class VehicleRentalApp:
    def __init__(self):
        self.conn = sqlite3.connect("rental_app.db")
        self.cursor = self.conn.cursor()
        self.create_tables()
        self.logged_in_user = None

    def create_tables(self):
        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS users (
                id INTEGER PRIMARY KEY,
                username TEXT UNIQUE,
                password TEXT
            )
        ''')
        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS vehicles (
                id INTEGER PRIMARY KEY,
                vehicle_id TEXT UNIQUE,
                vehicle_type TEXT,
                is_available BOOLEAN
            )
        ''')
        self.cursor.execute('''
            CREATE TABLE IF NOT EXISTS rentals (
                id INTEGER PRIMARY KEY,
                user_id INTEGER,
                vehicle_id INTEGER,
                start_date TEXT,
                end_date TEXT,
                FOREIGN KEY (user_id) REFERENCES users (id),
                FOREIGN KEY (vehicle_id) REFERENCES vehicles (id)
            )
        ''')
        self.conn.commit()

    def hash_password(self, password):
        sha = hashlib.sha512()
        sha.update(password.encode('utf-8'))
        return sha.hexdigest()

    def register_user(self, username, password):
        hashed_password = self.hash_password(password)
        try:
            self.cursor.execute("INSERT INTO users (username, password) VALUES (?, ?)", (username, hashed_password))
            self.conn.commit()
            return "Registration successful. You can now log in."
        except sqlite3.IntegrityError:
            return "Username already exists. Please choose another username."

    def login(self, username, password):
        hashed_password = self.hash_password(password)
        self.cursor.execute("SELECT * FROM users WHERE username=? AND password=?", (username, hashed_password))
        user = self.cursor.fetchone()
        if user:
            self.logged_in_user = user
            return "Login successful."
        else:
            return "Invalid username or password."

    def add_vehicle(self, vehicle_id, vehicle_type):
        try:
            self.cursor.execute("INSERT INTO vehicles (vehicle_id, vehicle_type, is_available) VALUES (?, ?, ?)",
                                (vehicle_id, vehicle_type, True))
            self.conn.commit()
            return f"{vehicle_type} vehicle added successfully."
        except sqlite3.IntegrityError:
            return "Vehicle ID already exists. Please choose another ID."

    def list_vehicles(self):
        self.cursor.execute("SELECT * FROM vehicles")
        vehicles = self.cursor.fetchall()
        if vehicles:
            print("\nVehicle List:")
            for vehicle in vehicles:
                availability = "Available" if vehicle[3] else "Not Available"
                print(f"Vehicle ID: {vehicle[1]}, Type: {vehicle[2]}, Status: {availability}")
        else:
            print("No vehicles found.")

    def rent_vehicle(self, vehicle_id, rental_days):
        if not self.logged_in_user:
            return "Please log in before renting a vehicle."

        self.cursor.execute("SELECT * FROM vehicles WHERE vehicle_id=?", (vehicle_id,))
        vehicle = self.cursor.fetchone()
        if not vehicle:
            return "Vehicle not found."
        if not vehicle[3]:
            return "Vehicle is not available for rent."

        start_date = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        end_date = (datetime.now() + timedelta(days=rental_days)).strftime("%Y-%m-%d %H:%M:%S")
        user_id = self.logged_in_user[0]

        try:
            self.cursor.execute("INSERT INTO rentals (user_id, vehicle_id, start_date, end_date) VALUES (?, ?, ?, ?)",
                                (user_id, vehicle[0], start_date, end_date))
            self.cursor.execute("UPDATE vehicles SET is_available=0 WHERE id=?", (vehicle[0],))
            self.conn.commit()
            return f"Vehicle {vehicle[2]} rented successfully for {rental_days} days."
        except sqlite3.IntegrityError:
            return "An error occurred while renting the vehicle."

    def return_vehicle(self, rental_id):
        if not self.logged_in_user:
            return "Please log in before returning a vehicle."

        self.cursor.execute("SELECT * FROM rentals WHERE id=?", (rental_id,))
        rental = self.cursor.fetchone()
        if not rental:
            return "Rental not found."

        start_date = datetime.strptime(rental[3], "%Y-%m-%d %H:%M:%S")
        end_date = datetime.strptime(rental[4], "%Y-%m-%d %H:%M:%S")
        rental_duration = (end_date - start_date).days
        rental_cost = self.calculate_rental_cost(rental_duration)

        try:
            self.cursor.execute("DELETE FROM rentals WHERE id=?", (rental_id,))
            self.cursor.execute("UPDATE vehicles SET is_available=1 WHERE id=?", (rental[2],))
            self.conn.commit()
            return f"Vehicle returned. Rental duration: {rental_duration} days, Total cost: {rental_cost} TL."
        except sqlite3.IntegrityError:
            return "An error occurred while returning the vehicle."

    def calculate_rental_cost(self, rental_duration):
        # Kiralama süresine göre fiyat hesaplaması
        daily_rate = 100  # Günlük fiyat
        return daily_rate * rental_duration

    def main(self):
        while True:
            print("\nVehicle Rental App")
            print("1. Register")
            print("2. Login")
            print("3. Add Vehicle")
            print("4. List Vehicles")
            print("5. Rent Vehicle")
            print("6. Return Vehicle")
            print("7. Logout")
            print("8. Exit")

            choice = input("Please select an option: ")

            if choice == "1":
                username = input("Username: ")
                password = input("Password: ")
                print(self.register_user(username, password))

            elif choice == "2":
                username = input("Username: ")
                password = input("Password: ")
                print(self.login(username, password))

            elif choice == "3":
                if self.logged_in_user:
                    vehicle_id = input("Vehicle ID: ")
                    vehicle_type = input("Vehicle Type: ")
                    print(self.add_vehicle(vehicle_id, vehicle_type))
                else:
                    print("Please log in before adding a vehicle.")

            elif choice == "4":
                self.list_vehicles()

            elif choice == "5":
                if self.logged_in_user:
                    vehicle_id = input("Vehicle ID to rent: ")
                    rental_days = int(input("Rental days: "))
                    print(self.rent_vehicle(vehicle_id, rental_days))
                else:
                    print("Please log in before renting a vehicle.")
            elif choice == "6":
                if self.logged_in_user:
                    rental_id = int(input("Rental ID to return: "))
                    print(self.return_vehicle(rental_id))
                else:
                    print("Please log in before returning a vehicle.")
            elif choice == "7":
                self.logged_in_user = None
                print("Logged out.")

            elif choice == "8":
                self.conn.close()
                break

if __name__ == "__main__":
    rental_app = VehicleRentalApp()
    rental_app.main()
```

Feel free to explore and customize this project to suit your learning goals and interests. Happy coding!



Happy vehicle renting!
