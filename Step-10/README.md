# Step 10: Lab Seating with 2D Arrays 🖥️

We've used 2D arrays to track data over time (attendance). Now, we'll use the same data structure to represent physical space: a computer lab. This step is about modeling a room layout, searching for available seats, and managing student placement.

---

### Part 10.1: The New Challenge - A Physical Space

**Goal:** Understand why a 2D array is the perfect tool for modeling a lab. A computer lab isn't a simple list; it's a grid of desks with rows and columns.

**Concept Focus:** **Spatial Data Modeling**. We need a data structure that can map directly to a physical layout.

**The Conceptual Model:**
A 3x4 lab can be perfectly represented by a 2D array of Strings. Each cell will hold the name of the student occupying that seat, or it will be "Empty".



```
          | Col 1   | Col 2   | Col 3   | Col 4   |
-----------------------------------------------------
Row 1     | "Empty" | "Empty" | "Empty" | "Empty" |
Row 2     | "Empty" | "Empty" | "Empty" | "Empty" |
Row 3     | "Empty" | "Empty" | "Empty" | "Empty" |
```

**The Problem:** We have the blueprint. Now we need to build the interface for a lab technician to interact with this seating chart.

---

### Part 10.2: The Lab Management Menu

**Goal:** Create a new, dedicated sub-menu for managing lab seating assignments.

**Concept Focus:** UI/UX; adding a clean entry point for a new application module.

#### Expected Output

```console
--- Main Menu ---
1. Student Management
2. Attendance Management
3. Lab Management         <-- New Option
...
0. Exit
Enter your choice: 3

--- Lab Seating Management ---
1. View Lab Layout
2. Assign a Seat Manually
3. Auto-Assign First Available Seat
4. Release a Student's Seat
0. Back to Main Menu
Enter your choice:
```

---

### Part 10.3: Visualizing the Empty Lab

**Goal:** Implement the "View Lab Layout" feature. This will be our eyes into the 2D array, showing the current status of every seat.

**Concept Focus:** **2D Array Traversal**. We'll use nested loops to iterate through every row and column to print the grid.

#### Expected Output

The program displays a clean, visual representation of the empty lab.

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

**The Problem:** The lab is empty. We need a way to place students into these seats.

---

### Part 10.4: Manual Seat Assignment & Error Handling

**Goal:** Implement the manual seat assignment, but make it robust. The system must prevent a user from assigning a seat that is already taken or a seat that doesn't exist.

**Concept Focus:** **Input Validation** and **Conditional Logic**. Before assigning a seat at `[row][col]`, we must check if the coordinates are valid and if that cell is currently "Empty".

#### Expected Output (Scenario 1: Successful Assignment)

```console
--- Lab Seating Management ---
Enter your choice: 2

Enter Student ID to assign a seat: 101
Enter Row number (1-3): 2
Enter Column number (1-4): 3

Seat at (Row 2, Col 3) assigned to Student ID 101.
```

#### Expected Output (Scenario 2: Seat Already Occupied)

```console
--- Lab Seating Management ---
Enter your choice: 2

Enter Student ID to assign a seat: 102
Enter Row number (1-3): 2
Enter Column number (1-4): 3

Error: Seat (Row 2, Col 3) is already occupied by Student ID 101.
Assignment failed.
```

#### Expected Output (Scenario 3: Invalid Coordinates)

```console
--- Lab Seating Management ---
Enter your choice: 2

Enter Student ID to assign a seat: 103
Enter Row number (1-3): 5
Enter Column number (1-4): 1

Error: Invalid seat location. Row 5 does not exist.
Assignment failed.
```

**The Problem:** Manual assignment is good for control, but it's slow. A lab technician often just wants to find the *next available seat* quickly.

---

### Part 10.5: Auto-Assignment (Searching the Grid)

**Goal:** Implement the "Auto-Assign" feature. The program will scan the grid and place the student in the very first empty seat it finds.

**Concept Focus:** **2D Array Search**. The logic is: "Start at `[Row 1, Col 1]`. Is it empty? If yes, assign the seat and stop. If no, move to `[Row 1, Col 2]` and check again. Continue this process row by row until an empty seat is found."

#### Expected Output

The program finds the first available seat (Row 1, Col 1), assigns it, and reports back.

```console
--- Lab Seating Management ---
Enter your choice: 3

Enter Student ID for auto-assignment: 104

Searching for the first available seat...
Success! Student ID 104 has been assigned to (Row 1, Col 1).
```

---

### Part 10.6: Releasing a Seat

**Goal:** Implement the ability to free up a seat, making it available for another student.

**Concept Focus:** **Searching and Updating**. The program will first search the entire grid to find which seat a specific student is in, then update that cell back to "Empty".

#### Expected Output

After releasing a seat, viewing the layout confirms that the seat is now available again.

```console
--- Lab Seating Management ---
Enter your choice: 4

Enter Student ID to release from seat: 101
Searching for student...
Found Student ID 101 at (Row 2, Col 3). Releasing seat.

--- Lab Seating Management ---
Enter your choice: 1

--- Computer Lab A Layout ---
+-------+------------+------------+------------+------------+
|       |   Col 1    |   Col 2    |   Col 3    |   Col 4    |
+-------+------------+------------+------------+------------+
| Row 1 | [ ID 104 ] | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 2 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  | <-- Seat is now empty
| Row 3 | [ Empty ]  | [ Empty ]  | [ Empty ]  | [ Empty ]  |
+-------+------------+------------+------------+------------+
```

---

### Part 10.7: Advanced - Ragged Arrays for Different Labs

**Goal:** Introduce a more advanced concept. What if the school has another lab with a different, irregular shape? We can model this using a **ragged array**, where each row can have a different number of columns.

**Concept Focus:** **Ragged Arrays**. This demonstrates the flexibility of 2D arrays in Java.

#### Expected Output

We add a new "Special Projects Lab" with an irregular layout. The program's "View Layout" logic is smart enough to handle this and display it correctly.

```console
--- Special Projects Lab B Layout ---
+-------+------------+------------+------------+
|       |   Col 1    |   Col 2    |   Col 3    |
+-------+------------+------------+------------+
| Row 1 | [ Empty ]  | [ Empty ]  | [ Empty ]  |
| Row 2 | [ Empty ]  | [ Empty ]  |            <-- Note: Row 2 is shorter
| Row 3 | [ Empty ]  | [ Empty ]  | [ Empty ]  |
+-------+------------+------------+------------+
```

**Success!** We have successfully used a 2D array to model, manage, and search a physical space. We've built robust assignment logic with error handling and even explored advanced structures like ragged arrays to handle more complex scenarios.
