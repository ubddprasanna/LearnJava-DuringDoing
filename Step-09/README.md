# Step 9: Tracking Attendance with 2D Arrays 🗓️

Our system can handle a student's static information (like their name and a single mark), but a real school is dynamic. We need to track data that changes over time, and the most fundamental example is **daily attendance**.

In this step, we will learn how to model data across two dimensions (students and time) using a powerful new structure: the **2D array**. This will allow us to build a complete attendance tracking module, from marking daily presence to generating analytical reports. We'll also make our code more professional by organizing it into dedicated "manager" units and using constants for configuration.

---
### Project Folder Structure at the End of This Step

We are expanding our architecture. We're adding a central `Config.java` to hold all our application-wide settings and a new `AttendanceManager.java` to contain all logic related to attendance.

```
SchoolManagementSystem/
├── .gitignore
├── README.md
├── SchoolManagementSystem.java
└── utils/
    ├── AttendanceManager.java
    ├── Config.java
    ├── GradeCalc.java
    └── StudentIO.java
```

---

### Part 9.1: The New Challenge - Storing Data in a Grid

**Task:** Before writing code, we must design our data structure. We need a way to hold attendance for every student, for every day of the term. A simple 1D array is just a list—it's not up to the task. We need a grid.

**The Problem It Solves:** This lays the foundation for the entire attendance module. Without the right data structure, the logic for marking and retrieving attendance would be incredibly complex and inefficient (imagine having to create a new array for every single day!).

**Reasoning & Engineer Thinking:**
The best data structure is one that naturally maps to the real-world problem. Attendance is a grid: you have a list of students (rows) and a list of days (columns). A **2D array** is a perfect digital representation of this grid. We will decide on a convention: `attendance[studentIndex][dayIndex]`. This clarity is crucial before writing any code. We'll also define configuration constants (`MAX_STUDENTS`, `DAYS_PER_TERM`) in a central place. Storing such values centrally means if the school term changes from 30 days to 40, we only need to make the change in one place.



**Hints:**
* Think of a 2D array like a spreadsheet. Each student gets their own row, and each day of the term gets its own column. The cell where they intersect holds the attendance status (`true` for Present, `false` for Absent).
* We'll create a new, dedicated file for all attendance-related logic (e.g., `AttendanceManager.java`). This continues our work from Step 8 of keeping our code organized and giving each unit a single responsibility.

**Expected Outcome:**
This is a planning and design step. There is no new console output. The outcome is your understanding of this conceptual model:

```
          | Day 0 | Day 1 | Day 2 | Day 3 | ... (up to DAYS_PER_TERM - 1)
-------------------------------------------------------------------------
Student 0 | false | false | false | false | ...
Student 1 | false | false | false | false | ...
Student 2 | false | false | false | false | ...
... (up to MAX_STUDENTS - 1)
```

**Verification:**
Can you draw this grid on paper? Can you explain what `attendance[5][10] = true;` would represent? (Answer: The student at index 5 was present on day 10).

---

### Part 9.2: The Attendance Management Menu

**Task:** As with our other modules, the first user-facing step is to create a clean and intuitive menu.

**The Problem It Solves:** This provides the user with a clear entry point to access all the new attendance features we are about to build.

**Reasoning & Engineer Thinking:**
A good user interface (UI) organizes features logically. All attendance-related actions should be grouped together, making the application easy to navigate.

**Hints:**
* You will add a new option to your main menu loop.
* This option will lead to a new sub-menu, similar in structure to your `Student Management` menu.

**Expected Outcome:**
The user can now navigate from the main menu into the new, (currently empty) attendance module.

```console
--- Main Menu ---
1. Student Management
2. Lab Management
3. Attendance Management  <-- New Option
0. Exit
Enter your choice: 3

--- Attendance Management ---
1. Mark Daily Attendance
2. View Full Attendance Grid
3. Get Student Attendance Report
4. List Absentees for a Day
0. Back to Main Menu
Enter your choice:
```

**Verification:**
Run your program and confirm that you can enter `3` to see the `Attendance Management` menu and then enter `0` to return to the `Main Menu`.

---

### Part 9.3: Marking Daily Attendance (The "Write" Operation)

**Task:** Implement the core feature: marking attendance for a specific day. The program will ask for a day and then loop through each student, asking the teacher to mark them as Present (`P`) or Absent (`A`).

**The Problem It Solves:** This is the fundamental "write" operation. It allows us to populate our 2D array with real data.

**Reasoning & Engineer Thinking:**
The logic requires targeting a specific cell in our grid. For a given `dayIndex`, we need to loop through every `studentIndex` and update the value at `attendance[studentIndex][dayIndex]`. We must be careful with user input: a teacher will think in terms of "Day 1", but our array uses "index 0". Our code must handle this translation. We will reuse our `SafeInputReader` from Step 8 to ensure the day is a valid number.

**Hints:**
* First, ask the user for the day (e.g., 1 to 30). Remember to convert this to a 0-based index before using it with the array.
* Then, loop through your students. For each student, display their name and prompt the user to enter 'P' or 'A'.
* Update the `boolean[][]` array accordingly (`true` for 'P', `false` for 'A').

**Expected Outcome:**
A clear, interactive process for the teacher to enter the day's attendance records.

```console
--- Attendance Management ---
Enter your choice: 1

Enter the day to mark attendance for (1-30): 2

--- Marking Attendance for Day 2 ---
Student: John Doe (ID: 101). Present or Absent? (P/A): p
Student: Jane Smith (ID: 102). Present or Absent? (P/A): a
Student: Peter Jones (ID: 103). Present or Absent? (P/A): p

Attendance for Day 2 has been recorded.
```

**Verification:**
At this point, you can't visually verify the data yet. The true verification will come in the next part when we build the "viewer."

---

### Part 9.4: Viewing the Full Grid (The "Read" Operation)

**Task:** Let's give ourselves a way to see the data. Implement the "View Full Attendance Grid" feature to display the entire 2D array in a formatted table.

**The Problem It Solves:** This provides essential feedback. It allows the user (and you, the developer) to verify that the data entered in the previous step was stored correctly.

**Reasoning & Engineer Thinking:**
Displaying a grid requires **nested loops**. The outer loop will iterate through the students (rows), and the inner loop will iterate through the days (columns). For each cell, we'll print a `P`, `A`, or a `-` if attendance hasn't been marked yet. We can reuse our `StudentPrinter` ideas from Step 8 to format this nicely.

**Hints:**
* The outer loop runs from `studentIndex = 0` to `studentCount - 1`.
* The inner loop runs from `dayIndex = 0` to `DAYS_PER_TERM - 1`.
* Inside the inner loop, you'll check the value of `attendance[studentIndex][dayIndex]` and print the appropriate character.

**Expected Outcome:**
A clean, bird's-eye view of the entire term's attendance record.

```console
--- Full Attendance Grid (Term 1) ---
+----------------------+-------+-------+-------+ ... +--------+
| Name                 | Day 1 | Day 2 | Day 3 | ... | Day 30 |
+----------------------+-------+-------+-------+ ... +--------+
| John Doe             |   P   |   P   |   -   | ... |   -    |
| Jane Smith           |   P   |   A   |   -   | ... |   -    |
| Peter Jones          |   A   |   P   |   -   | ... |   -    |
+----------------------+-------+-------+-------+ ... +--------+
(P = Present, A = Absent, - = Not Yet Marked)
```

**Verification:**
Does the grid accurately reflect the data you entered in Part 9.3? For Day 2, does it show P, A, P for the three students? If so, your "write" and "read" logic is working perfectly.

---

### Part 9.5: Individual Analysis (Row Traversal)

**Task:** The full grid is useful, but often a teacher needs a specific statistic, like "What is Jane Smith's attendance percentage?" Implement the "Get Student Attendance Report" feature.

**The Problem It Solves:** This transforms raw data (`true`/`false` values) into a meaningful, calculated insight (a percentage).

**Reasoning & Engineer Thinking:**
This introduces a new way to traverse our 2D array: iterating across a **single row**. The logic is:
1.  Get the `studentIndex` for the requested student.
2.  Loop from `dayIndex = 0` to `DAYS_PER_TERM - 1`.
3.  Inside the loop, inspect only `attendance[studentIndex][dayIndex]`.
4.  Count the number of `true` values.
5.  Calculate the percentage.
Remember to handle the "divide by zero" edge case if no attendance has been marked for that student yet!

**Hints:**
* You'll need a helper to find a student's array index based on their ID. You should have already built logic for this in Step 5 (Search) that you can adapt.
* Remember to use floating-point division (like in Step 6) to get an accurate percentage.

**Expected Outcome:**
A concise and informative report for a single student.

```console
--- Attendance Management ---
Enter your choice: 3

Enter the Student ID to check: 102
Searching for Jane Smith...

--- Attendance Report for Jane Smith ---
Days Marked So Far: 2
Days Present: 1
Days Absent: 1
Attendance Percentage: 50.0%
```

**Verification:**
Look at the full grid for Jane Smith. Manually count her 'P's and the total days marked. Does your manual calculation match the program's output?

---

### Part 9.6: Daily Analysis (Column Traversal)

**Task:** Now let's answer a different question: "Who was absent *today*?" Implement the "List Absentees for a Day" feature.

**The Problem It Solves:** This provides a practical, daily-use tool for teachers. It demonstrates the flexibility of our data structure to answer different kinds of questions.

**Reasoning & Engineer Thinking:**
This introduces the opposite traversal pattern: iterating down a **single column**. The logic is:
1.  Get the `dayIndex` from the user.
2.  Loop from `studentIndex = 0` to `studentCount - 1`.
3.  Inside the loop, inspect only `attendance[studentIndex][dayIndex]`.
4.  If the value is `false`, add that student's name to a list of absentees.
5.  Display the final list.

**Hints:**
* Again, be careful to convert the user's "Day" (e.g., 2) to a "day index" (e.g., 1).
* If no one was absent, make sure to print a clear message like "All students were present."

**Expected Outcome:**
A simple, actionable list for the teacher.

```console
--- Attendance Management ---
Enter your choice: 4

Enter the day to check for absentees (1-30): 2

--- Absentees on Day 2 ---
- Jane Smith (ID: 102)

End of list.
```

**Verification:**
Look at the full grid view for Day 2. Does the list of absentees generated by the program match what you see in the grid? Test it for Day 1 as well, where Peter Jones should be listed.

---

## Step 9 Reflection

**What You Learned:**
* You learned how to model two-dimensional data (like students over time) using a **2D array**.
* You mastered the core operations on a 2D array: writing to a specific cell (`[row][col]`), and reading from it.
* You learned three fundamental traversal patterns: iterating over the **entire grid** (nested loops), iterating over a **single row**, and iterating down a **single column**.
* You reinforced the importance of organizing code into dedicated "manager" units and using central configuration constants.

**Improvement to the System:**
Our application has taken a huge leap forward. It is no longer limited to storing static, one-dimensional data. It can now track and analyze information that evolves over time, making it much closer to a real-world information system.

**How This Helps in the Next Step:**
The ability to model 2D space is not limited to time. In the next step, we will use the exact same 2D array concept to model a *physical* space: a computer lab seating chart. The skills you just built—traversing, searching, and updating a grid—will be directly applicable.
