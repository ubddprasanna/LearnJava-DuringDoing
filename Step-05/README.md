Of course. Here is the detailed guide for Step 5, broken into progressive sub-parts. This guide focuses on building the core search, update, and delete functionalities, exploring the logic and trade-offs involved, and showing the expected console outputs without any Java code.

You can copy the entire content below and save it in a file named `STEP-05_CRUD_Operations.md`.

-----

````markdown
# Step 5: Search, Update, and Delete Functionality 🔍

Our application can now add and list students. In this step, we will build the remaining core features of data management: finding, updating, and removing a specific student from the system. This will involve implementing a fundamental search algorithm and making a key design decision about how to handle data removal.

---

### Part 5.1: Expanding the Student Menu

**Goal:** Our current student menu is too simple. To accommodate the new features, we first need to update our menu to include options for searching, updating, and deleting students.

**Concept Focus:** User Interface (UI) and User Experience (UX) design. A well-organized menu makes an application intuitive to use.

#### Expected Output

The Student Management menu is now more comprehensive, providing a clear path for all the new actions we're about to build.

```console
--- Main Menu ---
1. Student Management
...
Enter your choice: 1

--- Student Management ---
1. Add a New Student
2. List All Students
3. Search for a Student by ID
4. Update a Student's Marks
5. Delete a Student
0. Back to Main Menu
Enter your choice:
```

**The Problem:** The new menu options are just placeholders. Selecting them does nothing yet. Let's implement the most important one first: Search.

---

### Part 5.2: Finding a Student (Linear Search)

**Goal:** Implement the "Search for a Student" feature. The user will provide a student's ID, and the program will scan through the array to find and display that student's details.

**Concept Focus:** **Linear Search Algorithm**. This is the most straightforward search method. The logic is: "Start at the first item in the array (`index 0`) and inspect it. Is this the one I'm looking for? If yes, stop. If no, move to the next item (`index 1`) and repeat until you either find the item or have checked every item in the list."

#### Expected Output (Scenario 1: Successful Search)

The user enters an ID that exists. The program finds the student and displays their information in our clean, formatted table.

```console
... (Assume students 101, 102, and 103 have been added) ...

--- Student Management ---
Enter your choice: 3

Enter the Student ID to search for: 102
Searching...

--- Student Found ---
+---------+----------------------+-------+
| ID      | Name                 | Marks |
+---------+----------------------+-------+
| 102     | Jane Smith           | 92    |
+---------+----------------------+-------+
```

#### Expected Output (Scenario 2: Unsuccessful Search)

The user enters an ID that does not exist in our records. The program searches the entire list, finds no match, and reports back gracefully.

```console
--- Student Management ---
Enter your choice: 3

Enter the Student ID to search for: 999
Searching...

Error: Student with ID 999 not found.
```

**The Problem:** We can now find students, which is great! This is the foundation for both updating and deleting. Let's tackle Update first.

---

### Part 5.3: Updating a Student's Marks

**Goal:** Implement the "Update" feature. This will reuse our new search logic to find the student, and then prompt the user for the new marks.

**Concept Focus:** Code reuse, array element assignment (`marks[index] = newValue`).

#### Expected Output

The user finds a student and provides a new value for their marks. A "before and after" check of the student list confirms the update was successful.

```console
... (Initial state of Jane Smith) ...
--- Student List ---
+---------+----------------------+-------+
| ID      | Name                 | Marks |
+---------+----------------------+-------+
| 102     | Jane Smith           | 92    |
+---------+----------------------+-------+

--- Student Management ---
Enter your choice: 4

Enter the Student ID to update: 102
Found Student: Jane Smith (Current Marks: 92)
Enter new marks: 95
Student marks updated successfully!

--- Student Management ---
Enter your choice: 2

--- Student List ---
+---------+----------------------+-------+
| ID      | Name                 | Marks |
+---------+----------------------+-------+
| 102     | Jane Smith           | 95    | <-- Marks have been updated
+---------+----------------------+-------+
```

**The Problem:** We can create, read, and update. The final core operation is deleting. Deleting an item from an array isn't as simple as just "removing" it; we have to decide how to handle the empty space it leaves behind.

---

### Part 5.4: The Deletion Challenge (Shift vs. Tombstone)

**Goal:** Before we code the delete function, we must make a crucial design decision. How do we remove an element from our array?

**Concept Focus:** Data Structure Strategy.

1.  **The "Tombstone" Method:** We find the student at, say, `index 5` and "mark" it as deleted (e.g., set the `studentID` to `0` or the `studentName` to `null`).
    * **Pro:** Very fast. It's a single write operation.
    * **Con:** It creates "holes" in our array. Our `List All Students` logic would now need an extra `if` check to avoid printing empty rows. Our `Add Student` logic would become much more complex, as it would need to search for a hole to fill before adding to the end.

2.  **The "Shifting" Method:** We find the student at `index 5`. Then, we move the student from `index 6` into `index 5`, the student from `index 7` into `index 6`, and so on, until we have shifted every subsequent student one spot to the left. Finally, we decrease our `studentCount`.
    * **Pro:** The array remains a clean, contiguous block of data. There are no holes. This keeps our `List All` and `Add Student` logic simple and fast.
    * **Con:** The deletion itself can be slow if the array is huge, as it involves moving a lot of data.

**Our Decision:** For our system, data integrity and simple logic are more important than the micro-optimization of the delete operation. We will use the **"Shifting" Method**.

**The Problem:** The "Shifting" method is a destructive, irreversible action. A user could accidentally delete the wrong student. A good program should ask for confirmation before proceeding.

---

### Part 5.5: Implementing Deletion with Confirmation

**Goal:** Implement the "Delete a Student" feature using the shifting method, including a confirmation prompt to prevent accidental data loss.

**Concept Focus:** Reusing search logic, implementing the array shift, and user confirmation prompts.

#### Expected Output

The user lists students, chooses to delete one, and confirms the action. Afterward, listing the students again shows that the record has been completely removed and the array is clean.

```console
... (John, Jane, and Alexander are in the system) ...
--- Student List ---
+---------+----------------------+-------+
| ID      | Name                 | Marks |
+---------+----------------------+-------+
| 101     | John Doe             | 88    |
| 102     | Jane Smith           | 95    |
| 103     | Alexander Hamilton   | 75    |
+---------+----------------------+-------+

--- Student Management ---
Enter your choice: 5

Enter the Student ID to delete: 102
Found Student: Jane Smith. Are you sure you want to delete? (yes/no): yes

Student with ID 102 has been deleted.

--- Student Management ---
Enter your choice: 2

--- Student List ---
+---------+----------------------+-------+
| ID      | Name                 | Marks |
+---------+----------------------+-------+
| 101     | John Doe             | 88    |
| 103     | Alexander Hamilton   | 75    |
+---------+----------------------+-------+
```

**Success!** Our application now supports all the core "CRUD" (Create, Read, Update, Delete) operations. The student data can be effectively managed from creation to removal.
````
