from pathlib import Path

# Define the base directory as the current working directory
BASE_DIR = Path.cwd()
data_dir = BASE_DIR / "my_files"

# Define a subfolder where we'll keep data files
data_dir = BASE_DIR / 'my_files'

# Create the folder if it doesn't already exist
# exist_ok=True means: "don't throw an error if the folder is already there"
data_dir.mkdir(exist_ok=True)

print(f"Working in: {data_dir}")

#==============Task 1 — File Read & Write Basics================

print("\nTask 1: File Read & Write Basics")
# Part A — Write

# Step 1: creating python script using write mode ('w'). This will create the file if it doesn't exist, or overwrite it if it does.

with open("python_notes.txt", "w", encoding="utf-8") as file:
    file.write("Topic 1: Variables store data. Python is dynamically typed.\n")
    file.write("Topic 2: Lists are ordered and mutable.\n")
    file.write("Topic 3: Dictionaries store key-value pairs.\n")
    file.write("Topic 4: Loops automate repetitive tasks.\n")
    file.write("Topic 5: Exception handling prevents crashes.\n")

print("File written successfully")

# Step 2: Append two more lines of your own to the same file using append mode ('a'), which adds to the end of the file without overwriting existing content.

with open("python_notes.txt", "a", encoding="utf-8") as file:
    file.write("Topic 6: Functions help reuse code.\n")
    file.write("Topic 7: APIs help programs talk to each other.\n")

print("Lines appended successfully")

# Part B — Read

# Step 1: Read the file back and print each line numbered, stripping the trailing newline character (\n) from each line for cleaner output.
with open("python_notes.txt", "r", encoding="utf-8") as file:
    lines = file.readlines()

print("\n Your Notes:")

for i, line in enumerate(lines, start=1):
    print(f"{i}. {line.strip()}")

# Step 2: Count and print the total number of lines in the file.
print(f"\nTotal number of lines: {len(lines)}")

# Step 3: Ask the user for a keyword and print all lines that contain that keyword (case-insensitive). If no lines are found, print a friendly message.

keyword = input("\nEnter a keyword to search: ").lower()

found = False

for line in lines:
    if keyword in line.lower():
        print(line.strip())
        found = True

if not found:
    print("No matching lines found. Try a different keyword!")

print("\n=======End of Task 1========")

#==============Task 2 — API Integration================
print("\nTask 2: API Integration")

import requests

url = "https://dummyjson.com/products?limit=20"

# 1) Step 1 — Fetch and Display Products:
response = requests.get(url)
data = response.json()

products = data["products"]

print("ID  | Title                          | Category      | Price    | Rating")
print("----|--------------------------------|---------------|----------|-------")

for p in products:
    print(f"{p['id']:<4}| {p['title'][:30]:<30}| {p['category']:<13}| ${p['price']:<8}| {p['rating']}")

#Step 2 — Filter and Sort:
print("\nStep 2: Filter and Sort")

# Filter products with a rating ≥ 4.5.
filtered = [p for p in products if p["rating"] >= 4.5]

# Sort the filtered list by price in descending order
sorted_products = sorted(filtered, key=lambda x: x["price"], reverse=True)

#Print the filtered and sorted list.
print("\nTop Rated Products (Rating ≥ 4.5):")

for p in sorted_products:
    print(f"{p['title']} - ${p['price']} - Rating: {p['rating']}")

#Step 3 — Search by Category:
print("\nStep 3: Search by Category")

laptops = data["products"]

print("\n Laptops List:")

for laptop in laptops:
    print(f"{laptop['title']} - ${laptop['price']}")

#Step 4 — POST Request (Simulated):
print("\nStep 4: POST Request (Simulated)")

import requests

url = "https://dummyjson.com/products/add"

new_product = {
    "title": "My Custom Product",
    "price": 999,
    "category": "electronics",
    "description": "A product I created via API"
}

response = requests.post(url, json=new_product)

print("\n Server Response Status:", response.status_code)

# Step 1: Check if request was successful
if response.status_code == 200 or response.status_code == 201:
    try:
        data = response.json()
        print("JSON Response:")
        print(data)
    except ValueError:
        print("Response is not in JSON format")
        print(response.text)

else:
    print("Request failed!")
    print("Status Code:", response.status_code)
    print("Response Text:", response.text)

print("\n=======End of Task 2========")

#==============Task 3 — Exception Handling ================
print("\nTask 3: Exception Handling")

#Part A — Guarded Calculator:
print("\nPart A: Guarded Calculator")

def safe_divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return "Error: Cannot divide by zero"
    except TypeError:
        return "Error: Invalid input types"

# Test cases
#Below case shoud work, while the next two should trigger our error handling
print(safe_divide(10, 2))      # should work
print(safe_divide(10, 0))      # division by zero
print(safe_divide("ten", 2))     # invalid input type

#Part B — Guarded File Reader:
print("\nPart B: Guarded File Reader")

def read_file_safe(filename):
    try:
        with open(filename, "r", encoding="utf-8") as file:
            content = file.read()
            return content
    except FileNotFoundError:
        print(f"Error: File '{filename}' not found.")
    finally:
        print("File operation attempt complete.")

# Test case with "python_notes.txt" (should succeed) and "ghost_file.txt" (should fail).
print("\nReading python_notes.txt:")
print(read_file_safe("python_notes.txt"))

print("\nReading ghost_file.txt:")
print(read_file_safe("ghost_file.txt"))

#Part C — Robust API Calls:
print("\nPart C: Robust API Calls")
import requests

def fetch_products():
    url = "https://dummyjson.com/products?limit=20"

    try:
        response = requests.get(url, timeout=5)
        response.raise_for_status()

        data = response.json()
        return data["products"]

    except requests.exceptions.ConnectionError:
        print("Connection failed. Please check your internet.")

    except requests.exceptions.Timeout:
        print("Request timed out. Try again later.")

    except Exception as e:
        print("Something went wrong:", e)

    return []

def add_product():
    url = "https://dummyjson.com/products/add"

    new_product = {
        "title": "My Custom Product",
        "price": 999,
        "category": "electronics",
        "description": "A product I created via API"
    }

    try:
        response = requests.post(url, json=new_product, timeout=5)
        response.raise_for_status()

        print("\n Product Added:")
        print(response.json())

    except requests.exceptions.ConnectionError:
        print("Connection failed. Please check your internet.")

    except requests.exceptions.Timeout:
        print("Request timed out. Try again later.")

    except Exception as e:
        print("Something went wrong:", e)

#Part D — Input Validation Loop:
print("\nPart D: Input Validation Loop")

import requests

while True:
    user_input = input("\nEnter a product ID to look up (1–100), or 'quit' to exit: ")

    if user_input.lower() == "quit":
        print("Goodbye")
        break

    # Validate input
    if not user_input.isdigit():
        print("Please enter a valid number.")
        continue

    product_id = int(user_input)

    if product_id < 1 or product_id > 100:
        print("ID must be between 1 and 100.")
        continue

    url = f"https://dummyjson.com/products/{product_id}"

    try:
        response = requests.get(url, timeout=5)

        if response.status_code == 404:
            print("Product not found.")
        elif response.status_code == 200:
            data = response.json()
            print(f"{data['title']} - ${data['price']}")
        else:
            print("Unexpected response:", response.status_code)

    except requests.exceptions.ConnectionError:
        print("Connection failed. Please check your internet.")

    except requests.exceptions.Timeout:
        print("Request timed out. Try again later.")

    except Exception as e:
        print("Something went wrong:", e)


print("\n=======End of Task 3========") 
