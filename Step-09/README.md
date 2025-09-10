# Step 9: Tracking Attendance with 2D Arrays 🗓️

Our system can handle static student information, but a school is a dynamic environment. We need to track data over time, and the most fundamental example is attendance. This step introduces a powerful new data structure, the **2D array**, to solve this challenge.

---

### Part 9.1: The Limitation of 1D Arrays

**Goal:** First, let's understand why our current tools are not sufficient for this task. A single array can hold a list of items, but it can't represent data in two dimensions (like students *and* days).

**Concept Focus:** **Data Modeling**. How do we represent the relationship between a list of students and a series of dates? A simple list (`String[]`) or parallel lists are not enough.



**The Problem (A Thought Experiment):**
We need to track attendance for 3 students over a 5-day school week. How could we do this with our current arrays?
* `boolean[] day1Attendance`, `boolean[] day2Attendance`... This would be incredibly messy and inflexible. What happens when it's day 6? We'd have to add a new array and recompile the program.
* This approach is not scalable. We need a single structure that can hold all this information.

---

### Part 9.2: Visualizing the 2D Array (The Grid)

**Goal:** Introduce the solution: a **2D array**. Conceptually, it's not a list, but a **grid** or a **table** with rows and columns. This is a perfect fit for our problem.

**Concept Focus:** **2D Array Structure**.
* **Rows:** Each row will represent a single student.
* **Columns:** Each column will represent a single day of the term.
* **Cell:** The value at `[row][column]` (e.g., `attendance[studentIndex][dayIndex]`) will be `true` (Present) or `false` (Absent).

**The Conceptual Model:**

```
          | Day 1 | Day 2 | Day 3 | Day 4 | Day 5 |
-----------------------------------------------------
Student 1 | true  | true  | false | true  | true  |  <- Row 0
Student 2 | true  | true  | true  | true  | true  |  <- Row 1
Student 3 | false | false | true  | true  | false |  <- Row 2
```

**The Problem:** We have the blueprint. Now, we need to build the user interface to interact with this new data structure.

---

### Part 9.3: The Attendance Management Menu

**Goal:** Let's create a new sub-menu in our application dedicated to managing attendance.

**Concept Focus:** UI/UX; creating a clear entry point for our new features.

#### Expected Output

```console
--- Main Menu ---
1. Student Management
2. Attendance Management  <-- New Option
...
0. Exit
Enter your choice: 2

--- Attendance Management ---
1. Mark Attendance for a Day
2. View Full Attendance Grid
3. Check a Student's Attendance Percentage
0. Back to Main Menu
Enter your choice:
```

**The Problem:** The menu is in place, but the core functionality—marking a student as present or absent—is not yet implemented.

---

### Part 9.4: Marking Attendance

**Goal:** Implement the "Mark Attendance" feature. The user will specify a day and then go through the list of students to mark them as Present (`P`) or Absent (`A`).

**Concept Focus:** **Nested Loops** and **2D Array Access**. The logic is: "For a given day (column), loop through each student (row) and update the value in that cell (`attendance[studentIndex][dayIndex]`)."

#### Expected Output

The user experience is a simple, guided process for a specific day.

```console
--- Attendance Management ---
Enter your choice: 1

Enter the day to mark attendance for (e.g., 1-5): 3

--- Marking Attendance for Day 3 ---
Student: John Doe (ID: 101). Present or Absent? (P/A): p
Student: Jane Smith (ID: 102). Present or Absent? (P/A): p
Student: Peter Jones (ID: 103). Present or Absent? (P/A): a

Attendance for Day 3 has been recorded.
```

**The Problem:** We've stored the data, but how can we be sure it's correct? We need a way to view the entire attendance grid.

---

### Part 9.5: Displaying the Full Attendance Grid

**Goal:** Implement the "View Full Attendance Grid" feature. This will display our conceptual model as a clean, readable table in the console.

**Concept Focus:** **Nested Loops for Traversal**. To print a grid, you need an outer loop for the rows (students) and an inner loop for the columns (days).

#### Expected Output

After marking attendance for a few days, the full grid provides a complete overview of the class's attendance record.

```console
--- Attendance Management ---
Enter your choice: 2

--- Full Attendance Grid (Term 1) ---
+----------------------+-------+-------+-------+-------+-------+
| Name                 | Day 1 | Day 2 | Day 3 | Day 4 | Day 5 |
+----------------------+-------+-------+-------+-------+-------+
| John Doe             |   P   |   P   |   P   |   -   |   -   |
| Jane Smith           |   P   |   A   |   P   |   -   |   -   |
| Peter Jones          |   A   |   A   |   A   |   -   |   -   |
+----------------------+-------+-------+-------+-------+-------+
(P = Present, A = Absent, - = Not Yet Marked)
```

**The Problem:** The grid is great for a high-level view, but it's not a quick statistic. We need to be able to calculate an individual student's attendance percentage.

---

### Part 9.6: Calculating Attendance Percentage

**Goal:** Implement the feature to check a single student's attendance record and calculate their percentage.

**Concept Focus:** **Iterating a Single Row**. To calculate this, we don't need nested loops. We just need to find the student's row and then loop through the columns for that row only, counting the number of `P`'s.

#### Expected Output

The system provides a clear, concise report for a single student.

```console
--- Attendance Management ---
Enter your choice: 3

Enter the Student ID to check: 103

--- Attendance Report for Peter Jones ---
Days Marked: 3
Days Present: 0
Days Absent: 3
Attendance Percentage: 0.0%

WARNING: Attendance is critically low!
```

**Success!** We have successfully modeled and managed time-based data using a 2D array. We can record, view, and analyze attendance records, and even add conditional warnings based on the results. We have moved from simple lists to more complex, grid-like data structures.
