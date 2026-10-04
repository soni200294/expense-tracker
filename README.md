import json
import sys
from pathlib import Path

DATA_FILE = Path("expenses.json")


def load_expenses():
    if DATA_FILE.exists():
        return json.loads(DATA_FILE.read_text())
    return []


def save_expenses(expenses):
    DATA_FILE.write_text(json.dumps(expenses, indent=2))


def add_expense(name, amount):
    expenses = load_expenses()
    expenses.append({"name": name, "amount": float(amount)})
    save_expenses(expenses)


def total_expenses():
    return sum(e["amount"] for e in load_expenses())


def main():
    if len(sys.argv) < 2:
        print("Usage: python expense_tracker.py [add <name> <amount> | list | total]")
        return
    command = sys.argv[1]
    if command == "add":
        add_expense(sys.argv[2], sys.argv[3])
        print("Expense added!")
    elif command == "list":
        for e in load_expenses():
            print(f"{e['name']}: {e['amount']}")
    elif command == "total":
        print("Total:", total_expenses())


if __name__ == "__main__":
    main()
