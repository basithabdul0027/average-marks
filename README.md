!/usr/bin/env python3
"""
average_marks.py
Simple program to calculate student averages with input validation.
Saves results to student_averages.csv
"""

import csv

def get_positive_int(prompt):
    while True:
        try:
            v = int(input(prompt))
            if v <= 0:
                print("Please enter a positive integer.")
                continue
            return v
        except ValueError:
            print("That's not an integer. Try again.")

def get_mark(prompt):
    while True:
        try:
            m = float(input(prompt))
            if m < 0 or m > 100:
                print("Please enter a mark between 0 and 100.")
                continue
            return m
        except ValueError:
            print("Please enter a numeric value (like 72.5).")

def main():
    students = {}
    n = get_positive_int("Enter number of students: ")

  for i in range(n):
        name = input(f"\nEnter name of student {i+1}: ").strip()
        while not name:
            print("Name cannot be empty.")
            name = input(f"Enter name of student {i+1}: ").strip()

  subjects = get_positive_int(f"How many subjects for {name}? ")
        total = 0.0
        for j in range(subjects):
            mark = get_mark(f"Enter marks for subject {j+1}: ")
            total += mark
        average = total / subjects
        students[name] = average
    sorted_students = sorted(students.items(), key=lambda kv: kv[1], reverse=True)
     print("\nStudent Averages (highest -> lowest):")
    for name, avg in sorted_students:
        print(f"{name}: {avg:.2f}")
    if sorted_students:
        top_avg = sorted_students[0][1]
        toppers = [name for name, avg in sorted_students if avg == top_avg]
        print("\nTop student(s):")
        for t in toppers:
            print(f"{t} with average {top_avg:.2f}")
    csv_filename = "student_averages.csv"
    try:
        with open(csv_filename, "w", newline="") as f:
            writer = csv.writer(f)
            writer.writerow(["Name", "Average"])
            for name, avg in sorted_students:
                writer.writerow([name, f"{avg:.2f}"])
        print(f"\nSaved results to {csv_filename}")
    except Exception as e:
        print("Failed to write CSV:", e)

if __name__ == "__main__":
    main()
