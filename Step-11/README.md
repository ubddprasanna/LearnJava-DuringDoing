# Step 11: The Library Module & Bundled Data Records 📚

Our system is growing with several independent modules. Now, we'll build one of the most important: the Library. This step will present a new challenge: a single "book" has many pieces of related information (ID, title, availability, borrower, due date).

Managing all of these with separate parallel arrays is becoming clumsy and error-prone. While we will still use arrays for now, we will start thinking about each book as a single **"data bundle"** or **"record unit"**. This shift in thinking is crucial for building complex, maintainable systems. We will implement the complete lifecycle of a library book: adding it to the collection, borrowing it, and returning it, complete with late fee calculations.

---

### Project Folder Structure at the End of This Step

We add the final major module for Phase 1: the `LibraryManager`. Our `utils` folder now contains a complete set of tools for managing the core parts of our school.

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
    ├── LibraryManager.java
    ├── SafeInputReader.java
    └── StudentIO.java
```

---

### Part 11.1: The Problem with "Parallel" Thinking

**Task:** Before we build, let's analyze the problem. Imagine we want to store information about our library books. Using our old method, we would create many parallel arrays:

- `String[] bookIds`
- `String[] bookTitles`
- `boolean[] isAvailable`
- `String[] borrowedByStudentId`
- `int[] dueDay`

**The Problem It Solves:** Recognizing this as a problem is the first step to better design. This approach is fragile. What happens if we want to add a new piece of information, like the book's `genre`? We would have to create a _sixth_ parallel array and then painstakingly update every single function that touches books (add, search, sort, display, save, etc.). A single mistake could corrupt our entire dataset, linking the wrong genre to a book.

**Reasoning & Engineer Thinking:**
Data that belongs together should be kept together. A book's ID, title, and availability are not separate, independent facts; they are all properties of a single book. While we will still implement this with parallel arrays for now to master the logic, we must mentally treat the elements at `index 5` of all these arrays as a single, cohesive **book record**. This mental shift is the most important part of this step.

**Expected Outcome:**
This is a design and thought experiment. The outcome is your understanding that our old parallel array method is becoming a liability. It's time to find a better way to structure our data.

**Verification:**
Ask yourself: "If I wanted to add a `publicationYear` to each book, how many different parts of my code would I have to change with the parallel array method?" The long and risky list of answers is your verification that a better design is needed.

---

### Part 11.2: The Library Management Menu

**Task:** As with our other modules, let's create a clean user interface for the librarian.

**The Problem It Solves:** This provides a clear, organized entry point for all the new library features we're about to build.

**Reasoning & Engineer Thinking:**
A consistent UI/UX makes the application predictable and easy to use. The librarian will immediately understand how to navigate this new section because it follows the same pattern as the modules they already know.

**Hints:**

- Add a new "Library Management" option to your main menu.
- This option will lead to a new sub-menu dedicated to all library operations.

**Expected Outcome:**
The user can now navigate from the main menu into the new, (currently empty) library module.

```console
--- Main Menu ---
1. Student Management
2. Attendance Management
3. Lab Management
4. Library Management      <-- New Option
0. Exit
Enter your choice: 4

--- Library Management ---
1. Add a New Book
2. Borrow a Book
3. Return a Book
4. List All Books
5. List Available Books
0. Back to Main Menu
Enter your choice:
```

**Verification:**
Run your program, enter `4` to see the `Library Management` menu, and then enter `0` to confirm you can return to the `Main Menu`.

---

### Part 11.3: Adding a Book (Enforcing Data Integrity)

**Task:** Implement the "Add a New Book" feature. A key part of this is ensuring that no two books can have the same ID.

**The Problem It Solves:** This establishes the foundation of our library collection and introduces a critical concept: **data integrity**. Duplicate IDs could cause chaos, allowing a student to return the wrong copy of a book.

**Reasoning & Engineer Thinking:**
Before adding a new book, the system must first _search_ the existing `bookIds` array to see if the new ID already exists. This is a crucial validation step. Good software anticipates and prevents user error. We are building a system that is not just functional, but trustworthy.

**Hints:**

- Reuse the linear search logic you built in Step 5.
- If a duplicate ID is found, display a clear error message and cancel the "add" operation.
- If the ID is unique, proceed to ask for the book's title and add the new "book record" to the end of your parallel arrays. Remember to set its initial `isAvailable` status to `true`.

**Expected Outcome:**

**Scenario 1: Successful Add**

```console
--- Library Management ---
Enter your choice: 1

Enter new Book ID: B001
Enter Book Title: Introduction to Java
Success: "Introduction to Java" has been added to the library.
```

**Scenario 2: Duplicate ID Error**

```console
--- Library Management ---
Enter your choice: 1

Enter new Book ID: B001
Error: A book with ID B001 already exists.
```

**Verification:**
Add book `B001`. Then, try to add `B001` again and confirm you see the error message. Finally, add book `B002` and confirm it succeeds.

---

### Part 11.4: Borrowing a Book (Managing State)

**Task:** Implement the "Borrow a Book" feature. This is our first complex transaction that changes the state of a "data bundle."

**The Problem It Solves:** This brings the library to life. It models the core real-world interaction of a student checking out a book and establishes a due date for its return.

**Reasoning & Engineer Thinking:**
This is a multi-step process that must be executed carefully:

1.  **Identify:** Find the book and the student.
2.  **Verify State:** Check if the book is actually available. You cannot borrow a book that is already checked out.
3.  **Update State:** If it's available, you must perform several updates to the book's record: set `isAvailable` to `false`, record the `borrowedByStudentId`, and calculate the `dueDay`.
4.  **Provide Feedback:** Inform the user whether the transaction was successful or not.

**Hints:**

- We'll simulate time with simple integers. Ask the user for the "Current Day".
- Define a standard `LOAN_PERIOD` constant (e.g., 7 days). The `dueDay` is simply `currentDay + LOAN_PERIOD`.
- The `findBookIndexById` tool you will build is a crucial reusable helper.

**Expected Outcome:**

**Scenario 1: Successful Borrow**

```console
--- Library Management ---
Enter your choice: 2

Enter the Current Day (as a number): 10
Enter Book ID to borrow: B001
Enter your Student ID: STU101

Processing...
Book "Introduction to Java" is available.
Success: Book B001 borrowed by STU101. It is due on Day 17.
```

**Scenario 2: Book Already Checked Out**

```console
--- Library Management ---
Enter your choice: 2

Enter the Current Day (as a number): 11
Enter Book ID to borrow: B001
Enter your Student ID: STU102

Processing...
Error: Book "Introduction to Java" is currently borrowed by STU101.
```

**Verification:**
After a successful borrow, use the "List All Books" feature (which you'll build next) to inspect the book's state. Confirm that its status has changed to "Borrowed" and the correct student and due date are listed.

---

### Part 11.5: Returning a Book (Implementing Business Logic)

**Task:** Implement the "Return a Book" feature. This is the reverse of borrowing, but with added business logic: calculating late fees.

**The Problem It Solves:** This completes the book lifecycle and introduces the concept of implementing real-world "business rules" in code.

**Reasoning & Engineer Thinking:**
The core of this task is a conditional check.

1.  **Find:** Find the book's record using its ID.
2.  **Get Input:** Ask the user for the "Return Day".
3.  **Compare:** Compare the `returnDay` to the book's stored `dueDay`.
4.  **Execute Logic:**
    - If `returnDay <= dueDay`, the book is on time. Simply reset its state: `isAvailable = true`, `borrowedByStudentId = ""`, `dueDay = 0`.
    - If `returnDay > dueDay`, the book is late. First, calculate `lateDays = returnDay - dueDay`. Then, calculate a fine (e.g., `fine = lateDays * FINE_PER_DAY`). Display the fine, and _then_ reset the book's state.

**Hints:**

- Create a constant for `FINE_PER_DAY` (e.g., `0.50`).
- Use formatted printing to display currency values nicely.

**Expected Outcome:**

**Scenario 1: On-Time Return**

```console
--- Library Management ---
Enter your choice: 3

Enter the Return Day (as a number): 15
Enter Book ID to return: B001

Processing...
Book due on Day 17.
Success: Thank you for returning "Introduction to Java" on time.
```

**Scenario 2: Late Return**

```console
--- Library Management ---
Enter your choice: 3

Enter the Return Day (as a number): 20
Enter Book ID to return: B001

Processing...
Book was due on Day 17.
This book is 3 days late.
A late fee of $1.50 has been assessed.
Success: "Introduction to Java" has now been returned.
```

**Verification:**
After any return, use "List All Books" to confirm the book's status is now "Available".

---

### Part 11.6: Library Reports (Reading and Filtering State)

**Task:** Implement two key reports for the librarian: "List All Books" (showing their full status) and "List Available Books" (a filtered view).

**The Problem It Solves:** Raw data is not enough. The user needs well-formatted reports to understand the state of the library at a glance.

**Reasoning & Engineer Thinking:**
This demonstrates how we can **query and filter** our data. "List All" is a simple loop. "List Available" requires a loop with an `if` condition inside it to filter out the records we don't want to see. This is a fundamental concept in data processing.

**Expected Outcome:**
A comprehensive view that shows the status of every book, making it easy to see which books are out and when they are due.

```console
--- Library Management ---
Enter your choice: 4

--- Full Library Catalog ---
+-------+-------------------------+-------------+-------------+---------+
| ID    | Title                   | Status      | Borrowed By | Due Day |
+-------+-------------------------+-------------+-------------+---------+
| B001  | Introduction to Java    | Borrowed    | STU101      | 17      |
| B002  | Data Structures         | Available   | -           | -       |
| B003  | Algorithms in a Nutshell| Available   | -           | -       |
+-------+-------------------------+-------------+-------------+---------+
```

**Verification:**
Borrow book `B002`. Run the "List All Books" report and see its status change. Then run the "List Available Books" report and confirm that `B002` no longer appears on that specific list.

---

## Step 11 Reflection

**What You Learned:**

- You learned about the limitations of parallel arrays and the importance of thinking about related data as a single **"record unit"** or **"data bundle"**.
- You implemented a complete data lifecycle: Create (Add Book), Update (Borrow/Return), and Read (List Books).
- You learned how to manage **state** within your data (e.g., `isAvailable`).
- You implemented real-world **business logic** (calculating a due date and assessing late fees) using conditional statements.
- You practiced enforcing **data integrity** by preventing duplicate book IDs.

**Improvement to the System:**
Our school now has a functional library! The system is no longer just a set of disconnected modules; it's a more integrated platform that manages students, resources (labs), and now, educational materials (books).

**How This Helps in the Next Step:**
All the data in our library—every book, every borrower, every due date—is currently lost the moment we close the program. The system has no long-term memory. The next critical step, **File Handling**, will solve this problem by teaching us how to save our complex application state to a file and load it back again.
