# Task 1 — Data Parsing & Profile Cleaning
# Raw student data
raw_students = [
    {"name": "  ayesha SHARMA  ", "roll": "101", "marks_str": "88, 72, 95, 60, 78"},
    {"name": "ROHIT verma",       "roll": "102", "marks_str": "55, 68, 49, 72, 61"},
    {"name": "  Priya Nair  ",    "roll": "103", "marks_str": "91, 85, 88, 94, 79"},
    {"name": "karan MEHTA",       "roll": "104", "marks_str": "40, 55, 38, 62, 50"},
    {"name": " Sneha pillai ",    "roll": "105", "marks_str": "75, 80, 70, 68, 85"},
]

# Loop through each student and clean the data
for student in raw_students:
    # Clean the name by removing extra space and make Title Case
    clean_name = student["name"].strip().title()
    
    # Convert roll number from string to integer
    clean_roll = int(student["roll"])
    
    # Split marks string into list of integers
    clean_marks = [int(m) for m in student["marks_str"].split(", ")]
    
    # Check if name is valid (only alphabets in each word)
    valid_name = all(word.isalpha() for word in clean_name.split())
    
    # Print profile card
    print("================================")
    print(f"Student : {clean_name}")
    print(f"Roll No : {clean_roll}")
    print(f"Marks   : {clean_marks}")
    if valid_name:
        print("✓ Valid name")
    else:
        print("✗ Invalid name")
    print("================================")

# Print name in ALL CAPS and lowercase for roll 103
for student in raw_students:
    if int(student["roll"]) == 103:
        clean_name = student["name"].strip().title()
        print(f"\nRoll 103 Student Name in CAPS   : {clean_name.upper()}")
        print(f"Roll 103 Student Name in lower  : {clean_name.lower()}")

print("\n=======End of Task 1========")

# Task 2 — Marks Analysis Using Loops & Conditionals
# Step 1: Given data
student_name = "Ayesha Sharma"
subjects     = ["Math", "Physics", "CS", "English", "Chemistry"]
marks        = [88, 72, 95, 60, 78]

# Step 2: Function to get grade based on marks
def get_grade(m):
    if 90 <= m <= 100:
        return "A+"
    elif 80 <= m <= 89:
        return "A"
    elif 70 <= m <= 79:
        return "B"
    elif 60 <= m <= 69:
        return "C"
    else:
        return "F"

# Step 3: Print each subject with marks and grade
print(f"\nMarks Report for {student_name}")
print("--------------------------------")
for i in range(len(subjects)):
    print(f"{subjects[i]} : {marks[i]} ({get_grade(marks[i])})")

# Step 4: Calculate total, average, highest and lowest marks
total_marks = sum(marks)
average_marks = round(total_marks / len(marks), 2)
highest_index = marks.index(max(marks))
lowest_index  = marks.index(min(marks))

print("\nSummary")
print("--------------------------------")
print(f"Total Marks          : {total_marks}")
print(f"Average Marks        : {average_marks}")
print(f"Highest Scoring      : {subjects[highest_index]} ({marks[highest_index]})")
print(f"Lowest Scoring       : {subjects[lowest_index]} ({marks[lowest_index]})")

# Step 5: Give while loop for adding new subjects
new_count = 0
while True:
    subject = input("\nEnter subject name (or type 'done' to stop): ")
    if subject.lower() == "done":
        break
    
    mark_input = input(f"Enter marks for {subject} (0-100): ")
    
    # Step 6: Validate marks
    if not mark_input.isdigit():
        print("✗ Invalid input! Marks must be a number.")
        continue
    
    mark_value = int(mark_input)
    if mark_value < 0 or mark_value > 100:
        print("✗ Invalid marks! Must be between 0 and 100.")
        continue
    
    # Add subject and marks
    subjects.append(subject)
    marks.append(mark_value)
    new_count += 1
    print(f"✓ Added {subject} with marks {mark_value}")

# After loop ends
updated_average = round(sum(marks) / len(marks), 2)
print("\nFinal Report")
print("--------------------------------")
print(f"New subjects added   : {new_count}")
print(f"Updated Average Marks: {updated_average}")

print("\n=======End of Task 2========")

# Task 3 — Class Performance Summary

class_data = [
    ("Ayesha Sharma",  [88, 72, 95, 60, 78]),
    ("Rohit Verma",    [55, 68, 49, 72, 61]),
    ("Priya Nair",     [91, 85, 88, 94, 79]),
    ("Karan Mehta",    [40, 55, 38, 62, 50]),
    ("Sneha Pillai",   [75, 80, 70, 68, 85]),
]

# Store averages for summary later
averages = []

print("Name              | Average | Status")
print("----------------------------------------")

for name, marks in class_data:
    avg = round(sum(marks) / len(marks), 2)
    averages.append((name, avg))
    
    status = "Pass" if avg >= 60 else "Fail"
    
    # Print formatted row
    print(f"{name:<16} | {avg:7.2f} | {status}")

# Count pass/fail
pass_count = sum(1 for _, avg in averages if avg >= 60)
fail_count = len(averages) - pass_count

# Find topper
topper = max(averages, key=lambda x: x[1])

# Class average (average of averages)
class_avg = round(sum(avg for _, avg in averages) / len(averages), 2)

print("\nSummary")
print("----------------------------------------")
print(f"Students Passed : {pass_count}")
print(f"Students Failed : {fail_count}")
print(f"Class Topper    : {topper[0]} ({topper[1]:.2f})")
print(f"Class Average   : {class_avg:.2f}")

print("\n=======End of Task 3========")

# Task 4 — String Manipulation Utility

essay = "  python is a versatile language. it supports object oriented, functional, and procedural programming. python is widely used in data science and machine learning.  "

# Step 1: Strip leading/trailing spaces
clean_essay = essay.strip()
print("Step 1 - Clean Essay:")
print(clean_essay)

# Step 2: Convert to Title Case
title_case = clean_essay.title()
print("\nStep 2 - Title Case:")
print(title_case)

# Step 3: Count how many times 'python' appears (case-insensitive)
count_python = clean_essay.lower().count("python")
print("\nStep 3 - Count of 'python':")
print(count_python)

# Step 4: Replace 'python' with 'Python 🐍'
replaced = clean_essay.lower().replace("python", "Python 🐍")
print("\nStep 4 - Replace 'python':")
print(replaced)

# Step 5: Split into sentences
sentences = clean_essay.split(". ")
print("\nStep 5 - Split Sentences:")
print(sentences)

# Step 6: Print each sentence numbered
print("\nStep 6 - Numbered Sentences:")
for i, sentence in enumerate(sentences, start=1):
    # Add a period at the end if missing
    if not sentence.endswith("."):
        sentence += "."
    print(f"{i}. {sentence}")

print("\n=======End of Task 4========")
