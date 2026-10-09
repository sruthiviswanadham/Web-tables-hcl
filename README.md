# Web-tables-hcl
## Website:
https://assertqa.com/practice/webtables
## Testcases:

Test case

Task

Expected result

TC01

Print all column headings

All headings displayed

TC02

Print the first data row

First employee record displayed

TC03

Print the last data row

Last employee record displayed

TC04

Search for an employee by last name

Matching record displayed

TC05

Extract all email addresses

Every email printed

TC06

Find the employee with the highest Due amount

Matching employee identified

TC07

Verify that a particular website link exists

Pass or fail reported

TC08

Count data rows without counting the header

 ## Code:
 ```

from selenium import webdriver
from selenium.webdriver.common.by import By
import time
import re

driver = webdriver.Chrome()
driver.get("https://assertqa.com/practice/webtables")
time.sleep(3)

# Find the web table
table = driver.find_element(By.TAG_NAME, "table")
headers = table.find_elements(By.XPATH, ".//thead//th")
rows = table.find_elements(By.XPATH, ".//tbody/tr")

# TC01: Print all column headings
print("\nTC01: All Column Headings")
for header in headers:
    print(header.text)

# TC02: Print the first data row
print("\nTC02: First Data Row")
if rows:
    print(rows[0].text)
else:
    print("No data rows found")

# TC03: Print the last data row
print("\nTC03: Last Data Row")
if rows:
    print(rows[-1].text)
else:
    print("No data rows found")

# TC04: Search for an employee by last name
print("\nTC04: Search Employee by Last Name")
last_name = input("Enter last name to search: ")
found = False

for row in rows:
    cells = row.find_elements(By.TAG_NAME, "td")
    if any(cell.text.strip().lower() == last_name.strip().lower()
           for cell in cells):
        print("Matching Record:", row.text)
        found = True

if not found:
    print("Employee not found")

# TC05: Extract all email addresses
print("\nTC05: All Email Addresses")
email_index = next(
    (i for i, h in enumerate(headers)
     if "email" in h.text.lower()), -1
)

if email_index == -1:
    print("Email column not found")
else:
    for row in rows:
        cells = row.find_elements(By.TAG_NAME, "td")
        if len(cells) > email_index:
            print(cells[email_index].text)

# TC06: Find employee with highest Due amount
print("\nTC06: Employee with Highest Due")
due_index = next(
    (i for i, h in enumerate(headers)
     if "due" in h.text.lower()), -1
)

employees = []

if due_index == -1:
    print("Due column not found")
else:
    for row in rows:
        cells = row.find_elements(By.TAG_NAME, "td")
        if len(cells) > due_index:
            amounts = re.findall(
                r"-?\d+(?:\.\d+)?",
                cells[due_index].text.replace(",", "")
            )
            if amounts:
                employees.append((float(amounts[0]), row.text))

    if employees:
        highest = max(employees, key=lambda x: x[0])
        print("Highest Due Amount:", highest[0])
        print("Employee Record:", highest[1])
    else:
        print("No valid Due amounts found")

# TC07: Verify that a website link exists
print("\nTC07: Verify Website Link")
links = table.find_elements(By.XPATH, ".//a[@href]")

if links:
    print("PASS: Website link exists")
    for link in links:
        print("Link:", link.get_attribute("href"))
else:
    print("FAIL: Website link not found")

# TC08: Count data rows without header
print("\nTC08: Count Data Rows")
print("Total Data Rows:", len(rows))

time.sleep(10)
driver.quit()
```
## Output:
<img width="1920" height="1080" alt="Screenshot (545)" src="https://github.com/user-attachments/assets/c77959ea-5f06-4780-99b7-19fe7aa3e306" />

