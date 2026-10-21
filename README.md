
# Personal Expense Tracker with CSV

## Project Description
The Personal Expense Tracker is a Python command-line application that helps users record and manage daily expenses. It stores data in a CSV file so expenses remain saved after the program closes.

## Features
- Add new expenses
- View all expenses
- Filter expenses by category
- Filter expenses by month
- Calculate total spending per category
- Calculate grand total spending
- Find the biggest single expense
- Validate dates and amounts
- Automatically create the CSV file if it is missing

## Technologies Used
- Python 3.10 or newer
- csv module
- datetime module
- pathlib module
- collections module

## Project Files
- main.py: Main Python program
- expenses.csv: Expense records
- README.md: Project documentation

## How to Run
1. Install Python 3.10 or newer.
2. Open the project folder in VS Code.
3. Open the terminal.
4. Run the command:

   python main.py

5. Choose an option from the menu.

The program creates expenses.csv automatically if the file does not exist.

## CSV Format
The CSV file contains four columns:

date,category,description,amount

Example:
2026-10-14,Food,Groceries,475.75

## Testing
Test adding expenses, viewing records, filtering by category and month, calculating summaries, validating invalid inputs, and restarting the program to verify that data persists.

## Problem Fixed
One problem addressed was the missing CSV file error. The program checks whether expenses.csv exists and creates it with the correct header if necessary.

## Conclusion
This project demonstrates Python programming, CSV file handling, input validation, data filtering, and expense summary calculations.
