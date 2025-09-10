# Step 2: The Interactive Menu Loop 🔄

This step transforms our static welcome screen into a living, interactive application. We will build the main menu, teach it to listen for user commands, and handle both valid and invalid input.

---

### Part 2.1: The Never-Ending Menu

**Goal:** First, let's make the application persistent. We'll use a loop to continuously display the menu options so the program doesn't exit immediately after running.

**Concept Focus:** `while(true)` loop.

At this stage, the program will print the menu over and over without stopping. You will need to manually terminate it (e.g., by pressing `Ctrl+C` in your terminal). This demonstrates that the application is now in a continuous "running" state.

#### Expected Output

```text
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
(and so on, forever...)
```

---

### Part 2.2: Pausing for User Input

**Goal:** An infinite loop isn't very useful on its own. Now, let's make the program pause after showing the menu and wait for the user to tell it what to do.

**Concept Focus:** Using the `Scanner` class to read input from the console.

After printing the menu, the application will now display a prompt and the cursor will blink, waiting for you to type a number and press Enter. For now, it won't do anything with the number you enter; it will simply loop back and show the menu again.

#### Expected Output

```console
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 1  <-- User types '1' and presses Enter

--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 2  <-- User types '2' and presses Enter

--- Main Menu ---
(and so on...)
```

---

### Part 2.3: Implementing a Graceful Exit

**Goal:** A program that you can't exit gracefully is a trap. Let's implement the "Exit" option to properly terminate the application.

**Concept Focus:** `if` statements or a `switch` case to check the user's input, and a `break` or `return` statement to exit the loop.

Now, the program will inspect the number you enter. If you type any number other than `0`, the loop will continue as before. But if you type `0`, the program will print a goodbye message and terminate.

#### Expected Output (Scenario 1: Choosing a non-exit option)

```console
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 1  <-- User types '1'

--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice:
```

#### Expected Output (Scenario 2: Choosing to exit)

```console
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 0  <-- User types '0'
Thank you for using the School Management System. Goodbye!

C:\Projects\SchoolManagementSystem>  <-- Program has terminated
```

---

### Part 2.4: Acknowledging User Choices

**Goal:** The menu should provide feedback for other options, not just the exit command. Let's add placeholder messages for the other menu items.

**Concept Focus:** Expanding your `if-else-if` chain or adding more `case` statements to your `switch`.

When the user selects an option like "Student Management," the system will now print a confirmation message before showing the main menu again. This confirms the input was received and prepares us to add the actual module logic in the next steps.

#### Expected Output

```console
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 1  <-- User types '1'
Navigating to Student Management...

--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 3  <-- User types '3'
Navigating to Library Management...

--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice:
```

---

### Part 2.5: Handling Invalid Input

**Goal:** What happens if a user enters a number that isn't on the menu? A robust program should handle this gracefully instead of doing nothing.

**Concept Focus:** Using a `default` case in a `switch` statement or a final `else` block.

If the user types an unrecognized number (like `99` or `-5`), the program will now display a helpful error message and then re-display the menu, prompting them to try again. This makes the user interface much friendlier.

#### Expected Output

```console
--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 99  <-- User types an invalid number
Invalid choice. Please enter a number between 0 and 4.

--- Main Menu ---
1. Student Management
2. Teacher Management
3. Library Management
4. Reports
0. Exit
Enter your choice: 1  <-- User now types a valid number
Navigating to Student Management...

--- Main Menu ---
(and so on...)
```
