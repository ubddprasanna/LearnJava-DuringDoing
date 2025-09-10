Of course. Here is the detailed guide for Step 6, broken down into progressive sub-parts. This guide focuses on building the data analysis features, highlighting common pitfalls like integer division and edge cases, and showing only the expected console outputs without any Java code.

You can copy the entire content below and save it in a file named `STEP-06_Data_Analysis.md`.

-----

````markdown
# Step 6: Student Marks Analysis 📊

Our system is now a capable data manager, but raw data isn't knowledge. This step is about transforming that data into meaningful insights. We will build a "Student Analysis" module to calculate statistics like the class average, identify top performers, and get an overall sense of class performance.

---

### Part 6.1: The Analysis Sub-Menu

**Goal:** Just as we did for student management, we'll create a new, dedicated sub-menu for our analysis tools. This keeps the main menu clean and organizes our new features.

**Concept Focus:** UI/UX, menu-driven architecture.

#### Expected Output

The user selects the new "Student Analysis" option from the main menu and is taken to a new screen with options for various reports.

```console
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Student Analysis  <-- New Option
0. Exit
Enter your choice: 4

--- Student Analysis Menu ---
1. Calculate Class Average Mark
2. Find Highest and Lowest Marks
3. View Full Class Performance Report
0. Back to Main Menu
Enter your choice:
```

**The Problem:** The menu is ready, but the options are just wires leading nowhere. Let's build the first and most fundamental calculation: the class average.

---

### Part 6.2: The Integer Division Pitfall

**Goal:** Let's implement the "Calculate Class Average" feature. A common beginner mistake in many programming languages, including Java, is **integer division**, where the fractional part of a result is discarded. We will demonstrate this problem first to understand why it happens.

**Concept Focus:** **Integer Division**. When you divide one integer by another integer, the result will always be an integer. For example, in integer math, `10 / 3 = 3`, not `3.33`. To calculate an average, we first need the sum of all marks.

Let's assume we have three students with the following marks:
* John Doe: 88
* Jane Smith: 95
* Alexander Hamilton: 75
* **Total Sum:** 258
* **True Average:** 258 / 3 = 86.0

#### Expected Output

The program correctly calculates the sum (`258`) but then performs integer division (`258 / 3`), resulting in a slightly incorrect, truncated average.

```console
--- Student Analysis Menu ---
Enter your choice: 1

Calculating class average...
Total marks of all 3 students: 258
Class Average Mark: 86   <-- Incorrect! The decimal part is missing.
```

**The Problem:** The calculated average is wrong. While close, this loss of precision is a significant bug. Imagine this being used for financial calculations! We must ensure our calculations are accurate.

---

### Part 6.3: The Floating-Point Solution

**Goal:** Fix the average calculation bug by using floating-point arithmetic (numbers with decimal points, like `double` in Java).

**Concept Focus:** **Type Casting**. To force a floating-point division, we need to convert at least one of the integers in the operation (the sum or the count) into a floating-point number *before* the division happens. This is called casting. For example, `(double)258 / 3` becomes `258.0 / 3`, which correctly results in `86.0`.

#### Expected Output

By applying the fix, the program now calculates and displays the correct, precise average.

```console
--- Student Analysis Menu ---
Enter your choice: 1

Calculating class average...
Total marks of all 3 students: 258
Class Average Mark: 86.0  <-- Correct!
```

**The Problem:** An average is useful, but it doesn't tell the whole story. A class average of 86 could mean everyone scored an 86, or it could mean half the class scored 100 and the other half scored 72. We need to find the range of scores.

---

### Part 6.4: Finding the Highest and Lowest Marks

**Goal:** Implement the "Find Highest and Lowest Marks" feature. This involves iterating through the marks array and keeping track of the highest and lowest values seen so far.

**Concept Focus:** **Iteration and Comparison**. The logic is: "Start by assuming the first student's mark is both the highest and lowest. Then, loop through the rest of the students. If you find a mark higher than your current highest, update it. If you find one lower than your current lowest, update that."

#### Expected Output

The program correctly identifies the maximum and minimum scores from the list of students.

```console
--- Student Analysis Menu ---
Enter your choice: 2

Analyzing scores...
Highest Mark in the Class: 95
Lowest Mark in the Class: 75
```

**The Problem:** Our system works perfectly when there are students. But what if the teacher runs the report before adding anyone? Trying to calculate an average with zero students would mean dividing by zero, which crashes the program!

---

### Part 6.5: Handling the "No Students" Edge Case

**Goal:** Make our analysis tools robust by adding a check. If there are no students in the system, we should display a friendly message instead of attempting calculations and crashing.

**Concept Focus:** **Edge Case Handling / Defensive Programming**. Before performing any calculation, we must check: `if (studentCount == 0)`.

#### Expected Output

The user tries to run a report on an empty list. Instead of crashing, the program provides a clear, informative message.

```console
--- Student Analysis Menu ---
Enter your choice: 1

Error: Cannot perform analysis. There are no students in the system.
Please add students first.

--- Student Analysis Menu ---
Enter your choice:
```

**Success!** Our analysis tools are now accurate, insightful, and robust. The final step is to combine all these calculations into a single, convenient report.

---

### Part 6.6: The Full Performance Report

**Goal:** Implement the "View Full Class Performance Report" option to consolidate all our analytical data into one clean, comprehensive screen.

**Concept Focus:** Data Presentation and Code Reuse.

#### Expected Output

```console
--- Student Analysis Menu ---
Enter your choice: 3

--- Class Performance Report ---
+---------------------------+-------+
| Metric                    | Value |
+---------------------------+-------+
| Total Students            | 3     |
| Average Mark              | 86.0  |
| Highest Mark              | 95    |
| Lowest Mark               | 75    |
+---------------------------+-------+
```
````
