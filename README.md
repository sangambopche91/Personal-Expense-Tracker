
import csv
from datetime import datetime
from pathlib import Path
from collections import defaultdict

FILE = Path(__file__).with_name("expenses.csv")
FIELDS = ["date", "category", "description", "amount"]


def ensure_file():
    """Create the CSV file and header if it does not exist."""
    if not FILE.exists():
        with FILE.open("w", newline="", encoding="utf-8") as f:
            writer = csv.DictWriter(f, fieldnames=FIELDS)
            writer.writeheader()


def load_expenses():
    """Read saved expenses from the CSV file."""
    ensure_file()
    with FILE.open("r", newline="", encoding="utf-8") as f:
        return list(csv.DictReader(f))


def save_expense(expense):
    """Save one expense to the CSV file."""
    ensure_file()
    with FILE.open("a", newline="", encoding="utf-8") as f:
        writer = csv.DictWriter(f, fieldnames=FIELDS)
        writer.writerow(expense)


def get_date():
    while True:
        value = input("Date (YYYY-MM-DD, Enter for today): ").strip()

        if not value:
            return datetime.today().strftime("%Y-%m-%d")

        try:
            return datetime.strptime(
                value, "%Y-%m-%d"
            ).strftime("%Y-%m-%d")
        except ValueError:
            print("Invalid date. Please use YYYY-MM-DD.")


def get_amount():
    while True:
        value = input("Amount: ").strip()

        try:
            amount = float(value)
            if amount <= 0:
                print("Amount must be greater than zero.")
                continue
            return f"{amount:.2f}"
        except ValueError:
            print("Enter a valid number, such as 120.50.")


def add_expense():
    print("\n--- Add Expense ---")
    date = get_date()

    category = input("Category (Food/Travel/Study/Bills/Fun): ").strip()
    while not category:
        print("Category cannot be empty.")
        category = input("Category: ").strip()

    description = input("Description: ").strip()
    while not description:
        print("Description cannot be empty.")
        description = input("Description: ").strip()

    amount = get_amount()

    expense = {
        "date": date,
        "category": category.title(),
        "description": description,
        "amount": amount,
    }

    save_expense(expense)
    print("Expense saved successfully!")


def view_expenses(rows):
    if not rows:
        print("No expenses found.")
        return

    print("-" * 70)
    print(f"{'Date':<12} {'Category':<12} {'Description':<25} {'Amount':>10}")
    print("-" * 70)

    for row in rows:
        print(
            f"{row['date']:<12} "
            f"{row['category']:<12} "
            f"{row['description'][:25]:<25} "
            f"{float(row['amount']):>10.2f}"
        )

    print("-" * 70)


def filter_expenses():
    rows = load_expenses()

    print("\n1. Filter by category")
    print("2. Filter by month")
    choice = input("Choose filter: ").strip()

    if choice == "1":
        category = input("Enter category: ").strip().casefold()
        results = [
            row for row in rows
            if row["category"].casefold() == category
        ]

    elif choice == "2":
        month = input("Enter month (YYYY-MM): ").strip()

        try:
            datetime.strptime(month, "%Y-%m")
        except ValueError:
            print("Invalid month format. Use YYYY-MM.")
            return

        results = [
            row for row in rows
            if row["date"].startswith(month)
        ]

    else:
        print("Invalid choice.")
        return

    view_expenses(results)


def show_summary():
    rows = load_expenses()

    if not rows:
        print("No expenses available for summary.")
        return

    totals = defaultdict(float)
    grand_total = 0.0
    biggest = None

    for row in rows:
        amount = float(row["amount"])
        totals[row["category"]] += amount
        grand_total += amount

        if biggest is None or amount > float(biggest["amount"]):
            biggest = row

    print("\n--- Spending Summary ---")

    for category, total in sorted(totals.items()):
        print(f"{category:<15}: {total:.2f}")

    print(f"\nGrand Total: {grand_total:.2f}")

    print(
        f"Biggest Expense: {biggest['description']} "
        f"({float(biggest['amount']):.2f}) on {biggest['date']}"
    )


def main():
    ensure_file()

    while True:
        print("\n====== PERSONAL EXPENSE TRACKER ======")
        print("1. Add expense")
        print("2. View all expenses")
        print("3. Filter expenses")
        print("4. Spending summary")
        print("5. Quit")

        choice = input("Choose an option (1-5): ").strip()

        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses(load_expenses())
        elif choice == "3":
            filter_expenses()
        elif choice == "4":
            show_summary()
        elif choice == "5":
            print("Expenses saved. Goodbye!")
            break
        else:
            print("Invalid choice. Enter a number from 1 to 5.")


if __name__ == "__main__":
    main()
