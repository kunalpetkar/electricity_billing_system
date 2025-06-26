# electricity_billing_system
Here's a complete Python project you can run directly in Visual Studio Code (VS Code) for an Electricity Billing and Customer Management System. It uses basic Python, OOP, and JSON file storage, and will work in the terminal inside VS Code.
# main.py

import json
from customer import Customer
from billing import Bill

DATA_FILE = 'customers.json'

def load_customers():
    try:
        with open(DATA_FILE, 'r') as f:
            data = json.load(f)
            return [Customer.from_dict(c) for c in data]
    except (FileNotFoundError, json.JSONDecodeError):
        return []

def save_customers(customers):
    with open(DATA_FILE, 'w') as f:
        json.dump([c.to_dict() for c in customers], f, indent=4)

def add_customer(customers):
    cid = input("Enter Customer ID: ")
    name = input("Enter Customer Name: ")
    address = input("Enter Address: ")
    units = int(input("Enter Units Consumed: "))

    customer = Customer(cid, name, address, units)
    customers.append(customer)
    save_customers(customers)
    print("✅ Customer added successfully!")

def view_customers(customers):
    if not customers:
        print("⚠️ No customers found.")
        return
    for c in customers:
        print(f"ID: {c.customer_id} | Name: {c.name} | Address: {c.address} | Units: {c.units_consumed}")

def generate_bill(customers):
    cid = input("Enter Customer ID to Generate Bill: ")
    for c in customers:
        if c.customer_id == cid:
            bill = Bill(c.units_consumed)
            total = bill.calculate_bill()
            print(f"\n🧾 Bill for {c.name}")
            print(f"Units Consumed: {c.units_consumed}")
            print(f"Total Amount: ₹{total}\n")
            return
    print("❌ Customer not found.")

def main():
    customers = load_customers()

    while True:
        print("\n========== Electricity Billing System ==========")
        print("1. Add New Customer")
        print("2. View Customers")
        print("3. Generate Bill")
        print("4. Exit")
        choice = input("Enter your choice: ")

        if choice == '1':
            add_customer(customers)
        elif choice == '2':
            view_customers(customers)
        elif choice == '3':
            generate_bill(customers)
        elif choice == '4':
            print("Exiting... Goodbye!")
            break
        else:
            print("❌ Invalid option. Try again.")

if __name__ == "__main__":
    main()
