# Car-rental

#imports and CarRental class
import sys
import datetime

class CarRental:
    def __init__(self, stock=0):
        self.stock = stock

    def display_available_cars(self):
        print(f"Available cars: {self.stock}")
        return self.stock

    def rent_hourly(self, num_cars):
        if num_cars <= 0:
            print("Number of cars should be positive.")
            return None
        elif num_cars > self.stock:
            print(f"Sorry, only {self.stock} cars are available.")
            return None
        else:
            now = datetime.datetime.now()
            print(f"Rented {num_cars} car(s) on hourly basis at {now.hour}:{now.minute}.")
            self.stock -= num_cars
            return now

    def rent_daily(self, num_cars):
        if num_cars <= 0:
            print("Number of cars should be positive.")
            return None
        elif num_cars > self.stock:
            print(f"Sorry, only {self.stock} cars are available.")
            return None
        else:
            now = datetime.datetime.now()
            print(f"Rented {num_cars} car(s) on daily basis at {now.date()}.")
            self.stock -= num_cars
            return now

    def rent_weekly(self, num_cars):
        if num_cars <= 0:
            print("Number of cars should be positive.")
            return None
        elif num_cars > self.stock:
            print(f"Sorry, only {self.stock} cars are available.")
            return None
        else:
            now = datetime.datetime.now()
            print(f"Rented {num_cars} car(s) on weekly basis starting {now.date()}.")
            self.stock -= num_cars
            return now

    def return_cars(self, request):
        rental_time, rental_basis, num_cars = request
        if rental_time and rental_basis and num_cars:
            self.stock += num_cars
            now = datetime.datetime.now()
            rental_period = now - rental_time

            bill = 0
            if rental_basis == 1:  # hourly
                hours = rental_period.total_seconds() / 3600
                bill = round(5 * hours * num_cars)
            elif rental_basis == 2:  # daily
                days = rental_period.total_seconds() / (3600 * 24)
                bill = round(20 * days * num_cars)
            elif rental_basis == 3:  # weekly
                weeks = rental_period.total_seconds() / (3600 * 24 * 7)
                bill = round(60 * weeks * num_cars)

            print(f"Thanks for returning your car(s). Total bill: ${bill}")
            return bill
        else:
            print("Invalid return. Missing information.")
            return None
#customer class
class Customer:
    def __init__(self):
        self.rental_time = None
        self.rental_basis = None
        self.num_cars = 0

    def request_car(self):
        try:
            cars = int(input("How many cars would you like to rent? "))
            if cars <= 0:
                print("Number of cars must be greater than 0.")
                return 0
            else:
                self.num_cars = cars
                return self.num_cars
        except ValueError:
            print("Invalid input. Please enter a valid number.")
            return 0

    def return_car(self):
        if self.rental_time and self.rental_basis and self.num_cars:
            return self.rental_time, self.rental_basis, self.num_cars
        else:
            return 0, 0, 0
#The main application loop
def main():
    rental_system = CarRental(20)  # Initial stock of 20 cars
    customer = Customer()

    while True:
        print("""
        ====== Car Rental Menu ======
        1. Display available cars
        2. Request car on hourly basis ($5/hour)
        3. Request car on daily basis ($20/day)
        4. Request car on weekly basis ($60/week)
        5. Return car
        6. Exit
        """)

        try:
            choice = int(input("Enter choice (1-6): "))
        except ValueError:
            print("Invalid input. Please enter a number between 1 and 6.")
            continue

        if choice == 1:
            rental_system.display_available_cars()

        elif choice == 2:
            customer.num_cars = customer.request_car()
            customer.rental_time = rental_system.rent_hourly(customer.num_cars)
            customer.rental_basis = 1

        elif choice == 3:
            customer.num_cars = customer.request_car()
            customer.rental_time = rental_system.rent_daily(customer.num_cars)
            customer.rental_basis = 2

        elif choice == 4:
            customer.num_cars = customer.request_car()
            customer.rental_time = rental_system.rent_weekly(customer.num_cars)
            customer.rental_basis = 3

        elif choice == 5:
            request = customer.return_car()
            rental_system.return_cars(request)
            customer.rental_basis, customer.rental_time, customer.num_cars = None, None, 0

        elif choice == 6:
            print("Thank you for using the car rental system!")
            break

        else:
            print("Please choose a valid option (1-6).")
#Running the main program
main()
        ====== Car Rental Menu ======
        1. Display available cars
        2. Request car on hourly basis ($5/hour)
        3. Request car on daily basis ($20/day)
        4. Request car on weekly basis ($60/week)
        5. Return car
        6. Exit
        
Available cars: 20

        ====== Car Rental Menu ======
        1. Display available cars
        2. Request car on hourly basis ($5/hour)
        3. Request car on daily basis ($20/day)
        4. Request car on weekly basis ($60/week)
        5. Return car
        6. Exit
        
Rented 1 car(s) on hourly basis at 14:35.

        ====== Car Rental Menu ======
        1. Display available cars
        2. Request car on hourly basis ($5/hour)
        3. Request car on daily basis ($20/day)
        4. Request car on weekly basis ($60/week)
        5. Return car
        6. Exit
        
Rented 1 car(s) on weekly basis starting 2025-04-16.

        ====== Car Rental Menu ======
        1. Display available cars
        2. Request car on hourly basis ($5/hour)
        3. Request car on daily basis ($20/day)
        4. Request car on weekly basis ($60/week)
        5. Return car
        6. Exit
        
