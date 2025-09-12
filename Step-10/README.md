# Step 10: Lab Seating & Seat Allocation with 2D Arrays 🖥️

We have successfully used 2D arrays to model data over *time* (attendance). In this step, we will discover their versatility by using the exact same structure to model *physical space*: a computer lab seating chart.

This module will bring our school to life, allowing us to manage a physical resource. We will build a complete lab management system, from visualizing the room layout to automatically assigning the first available seat. The skills you learned in the last step—traversing and updating a grid—will be directly applied in this new and exciting context.

---

### Project Folder Structure at the End of This Step

We continue to expand our modular design by adding a `LabManager.java` for all seating logic and a `LabsConfig.java` to define the layouts of our rooms.

```
SchoolManagementSystem/
├── .gitignore
├── README.md
├── SchoolManagementSystem.java
└── utils/
    ├── AttendanceManager.java
    ├── Config.java
    ├── GradeCalc.java
    ├── LabManager.java
    ├── LabsConfig.java
    └── StudentIO.java
```
---

### Part 10.1: The New Challenge - Modeling a Physical Space

**Task:** Before writing code, we must design our data model. We need a way to represent a computer lab with rows and columns of desks, and we need this design to be flexible enough to handle multiple labs of different sizes.

**The Problem It Solves:** This establishes a clear and intuitive connection between a real-world object (a room) and a programming structure (a 2D array). By planning for multiple labs from the start, we build a scalable system instead of a one-off solution.

**Reasoning & Engineer Thinking:**
The best data structure is one that naturally maps to the real-world problem. A lab is a natural grid. The best data structure is a **2D array of `String`s**, where each cell holds the ID of the student sitting there. An empty string (`""`) is a perfect, simple indicator for a vacant seat.

To handle multiple labs, we won't hardcode the size. Instead, we'll create a central configuration (e.g., in `LabsConfig.java`) that maps a lab's name to its dimensions. For example: `"Computer Lab A" -> {3, 4}` (3 rows, 4 columns). This makes our system data-driven; to add a new lab, we'd only need to update the configuration, not the core logic.



**Hints:**
* Create a new, organized file for your lab logic (e.g., `LabManager.java`).
* The 2D array might be declared like `String[][] seatMap`.

**Expected Outcome:**
This is a design phase. The outcome is your understanding of the conceptual model for a 3x4 lab:

```
          | Col 0      | Col 1      | Col 2      | Col 3      |
------------------------------------------------------------------
Row 0     | ""         | ""         | ""         | ""         |
Row 1     | ""         | ""         | ""         | ""         |
Row 2     | ""         | ""         | ""         | ""         |
```

**Verification:**
Can you explain what `seatMap[1][2] = "STU101";` would represent in the real-world lab? (Answer: The student with ID "STU101" is sitting in the desk at the 2nd row, 3rd column).

---

### Part 10.2: The Lab Management Menu

**Task:** Create a clean user interface to access the new lab management features.

**The Problem It Solves:** This provides a clear, organized entry point for the user, grouping all related lab functions in one place.

**Reasoning & Engineer Thinking:**
A consistent menu structure is key to a good user experience. This new sub-menu will follow the same pattern as our previous Student and Attendance menus, making the application feel familiar and intuitive.

**Hints:**
* Add a "Lab Management" option to your main menu loop.
* This option will lead to a new sub-menu dedicated to lab operations.

**Expected Outcome:**
The user can now navigate from the main menu into the new, (currently empty) lab module.

```console
--- Main Menu ---
1. Student Management
2. Attendance Management
3. Lab Management         <-- New Option
0. Exit
Enter your choice: 3

--- Lab Seating Management ---
1. View Lab Layout
2. Auto-Assign First Available Seat
3. Release a Student's Seat
0. Back to Main Menu
Enter your choice:
```

**Verification:**
Run your program and confirm that you can enter `3` to see the `Lab Seating Management` menu and then enter `0` to return to the `Main Menu`.

---

### Part 10.3: Visualizing the Lab (The `printSeatMap` Tool)

**Task:** Implement the "View Lab Layout" feature. This will be our eyes into the 2D array, showing the current status of every seat in a clean, formatted grid.

**The Problem It Solves:** A visual representation of data is essential for both the user and the developer. It provides immediate feedback and is crucial for verifying that our other functions (like assigning a seat) are working correctly.

**Reasoning & Engineer Thinking:**
This task highlights a critical concept: **separating the user's view from the program's internal model**. Users think of rows as "Row 1, Row 2...", but arrays use 0-based indices ("index 0, index 1..."). Our `printSeatMap` tool must perform this translation, showing 1-based coordinates to be user-friendly. Getting this wrong is a classic **off-by-one error**.

**Hints:**
* Use **nested loops** to iterate through the 2D array.
* Print numbered column and row headers.
* Inside the loop, check if the cell `seatMap[row][col]` is an empty string. If it is, print `[ Empty ]`; otherwise, print the student's ID.
* Remember to add 1 to your loop variables (`row` and `col`) when printing the headers to the user.

**Expected Outcome:**
A clean, visual representation of the empty lab, with user-friendly 1-based coordinates.

```console
--- Lab Seating Management ---
Enter your choice: 1

--- Computer Lab A Layout ---
+-------+------------+------------+------------+------------+
|       |   Col 1    |   Col 2    |   Col 3    |   Col 4    |
+-------+------------+------------+------------+------------+
| Row 1 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 2 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 3 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
+-------+------------+------------+------------+------------+
```

**Verification:**
Check that the dimensions of the printed grid (3 rows, 4 columns) match your configuration. Confirm that the headers are 1-based.

---

### Part 10.4: Auto-Assignment (Searching the Grid)

**Task:** Implement the "Auto-Assign" feature. The program must scan the grid and place a student in the very first empty seat it finds.

**The Problem It Solves:** Manual assignment is slow and requires the user to know which seats are free. This automates the process, making it fast and efficient for a lab technician.

**Reasoning & Engineer Thinking:**
This requires a **grid search algorithm**. The simplest approach is a linear scan: start at `[0][0]`, then `[0][1]`, and so on, row by row. The first cell containing `""` is our target. We'll build this as two helpers:
1.  `findFirstFreeSeat`: This tool does the search and returns the coordinates (e.g., `{row, col}`) or an indicator that the lab is full (e.g., `{-1, -1}`).
2.  `allocateSeat`: This is the public-facing tool. It calls `findFirstFreeSeat` and, if a seat is found, updates the `seatMap` with the student's ID.

**Hints:**
* Use nested loops to scan the `seatMap`.
* As soon as you find a cell where `seatMap[row][col].equals("")`, you've found your seat. You should immediately `return` the coordinates to stop the search.
* If the loops complete without finding an empty seat, the lab is full.

**Expected Outcome:**
The user successfully auto-assigns a student. Viewing the map afterward confirms the assignment.

```console
--- Lab Seating Management ---
Enter your choice: 2

Enter Student ID for auto-assignment: STU101

Searching for the first available seat...
Success! Student STU101 has been assigned to (Row 1, Col 1).

--- Lab Seating Management ---
Enter your choice: 1

--- Computer Lab A Layout ---
+-------+------------+------------+------------+------------+
|       |   Col 1    |   Col 2    |   Col 3    |   Col 4    |
+-------+------------+------------+------------+------------+
| Row 1 | [ STU101 ] | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 2 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 3 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
+-------+------------+------------+------------+------------+
```

**Verification:**
Run the auto-assign feature three times in a row with different student IDs. Verify that the students are placed at `(1,1)`, `(1,2)`, and `(1,3)` respectively. Then, test what happens when you try to assign a seat when the lab is full; the system should report an error gracefully.

---

### Part 10.5: Releasing a Seat (Searching and Updating)

**Task:** Implement the "Release a Student's Seat" feature, which makes a seat available again.

**The Problem It Solves:** This completes the management lifecycle of a lab seat. Seats are not permanently assigned; they must be freed up when a student leaves.

**Reasoning & Engineer Thinking:**
This is another grid search, but with a different goal. We aren't looking for an empty seat; we are looking for a specific `studentId`. This means we **must search the entire grid**, because the student could be sitting anywhere. Once we find the matching cell, we update its value back to `""`.

**Hints:**
* Use nested loops to scan the entire `seatMap`.
* Inside the loop, check if `seatMap[row][col].equals(studentIdToFind)`.
* If you find a match, set that cell to `""` and inform the user.
* What if the student isn't found? After the loops finish, if you haven't found the student, you should report that they were not in the lab.

**Expected Outcome:**
The user successfully releases a student's seat. The map is updated correctly.

```console
... (STU101 is at Row 1, Col 1) ...
--- Lab Seating Management ---
Enter your choice: 3

Enter Student ID to release from seat: STU101

Searching for student...
Found Student STU101 at (Row 1, Col 1). Releasing seat.
Seat has been freed.

--- Lab Seating Management ---
Enter your choice: 1

--- Computer Lab A Layout ---
+-------+------------+------------+------------+------------+
|       |   Col 1    |   Col 2    |   Col 3    |   Col 4    |
+-------+------------+------------+------------+------------+
| Row 1 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 2 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 3 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
+-------+------------+------------+------------+------------+
```

**Verification:**
Assign a student to a seat in the middle of the lab (e.g., `(2,2)`). Then release that student. View the map and confirm that only the seat at `(2,2)` became empty.

---

## Step 10 Reflection

**What You Learned:**
* You learned how to apply the **2D array** structure to model a physical space, proving its versatility beyond just tracking data over time.
* You implemented fundamental **grid search algorithms**: one that stops at the first match (`findFirstFreeSeat`) and one that must check every cell (`freeSeat`).
* You mastered the critical engineering task of **translating between an internal data model (0-based) and a user-friendly view (1-based)**, avoiding common off-by-one errors.
* You practiced building a configurable, data-driven system by defining lab layouts in a central configuration spot.

**Improvement to the System:**
Our School Management System can now manage a physical resource. It has a new, fully functional module for lab technicians, complete with visualization, automated assignment, and seat release capabilities.

**How This Helps in the Next Step:**
Our modules are becoming more interconnected. A key insight is that our data is currently not linked. If we delete a student from the Student Manager, their lab seat remains assigned to them—a "ghost" allocation. The next logical step is to create connections between these modules. For example, when a student is deleted, the system should automatically call our new `freeSeat` tool. This is the first step towards building a truly integrated system.
