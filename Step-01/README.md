# Step 1: Project Bootstrap & Welcome Screen 💧

## Overview

This is the foundational step, the "tiny water drop" of our project. The goal is to create the project's folder structure, write the simplest possible Java program to ensure your environment is working, and establish professional habits from the very beginning using Git for version control.

---

## Learning Objectives

- ✅ Create and navigate directories using the command line.
- ✅ Understand the critical Java rule: the `public class` name must match the `.java` filename.
- ✅ Write the code for the `main` method, the entry point of every Java application.
- ✅ Use `System.out.println()` to display text output.
- ✅ Master the fundamental compile-and-run cycle (`javac` and `java`).
- ✅ Initialize a Git repository to start tracking project history.

---

## Prerequisites

1.  **Java Development Kit (JDK):** You must have a JDK (version 8 or newer) installed. To check, open your terminal and run `java -version`.
2.  **Text Editor or IDE:** Any text editor like VS Code, Sublime Text, Atom, or even Notepad will work. An IDE like IntelliJ IDEA or Eclipse is also great.
3.  **Git:** You need Git installed to follow the "Pro-Engineer Checklist." To check, run `git --version`.

---

## Step-by-Step Instructions

#### 1. Create the Project Directory

First, we'll create a dedicated folder for our project. This keeps all related files organized. Open your terminal or command prompt and execute the following commands:

```bash
# Create a new directory named SchoolManagementSystem
mkdir SchoolManagementSystem

# Navigate into the newly created directory
cd SchoolManagementSystem
```

Your terminal prompt should now show that you are inside the `SchoolManagementSystem` folder. All subsequent commands in this guide should be run from this location.

#### 2. Create the Java Source File

Now, create the Java file where you'll write the code.

**Crucial Naming Rule:** In Java, if a class is declared as `public`, the file name **must** exactly match the class name, followed by the `.java` extension. Our main class will be `SchoolManagementSystem`, so the file must be named `SchoolManagementSystem.java`.

Create the file using your text editor or by running this command:

```bash
# For macOS/Linux
touch SchoolManagementSystem.java

# For Windows Command Prompt
echo. > SchoolManagementSystem.java
```

#### 3. Write the Welcome Screen Code

Open `SchoolManagementSystem.java` in your text editor and type or paste the following code into it.

```java
public class SchoolManagementSystem {
  public static void main(String[] args) {
    // The \n character creates a new line in the output
    System.out.println("Welcome to CyberCrewz Academy\nSchool Management System V1\nCode name: River");
  }
}
```

Save the file.

---

## Understanding the Code

- `public class SchoolManagementSystem { ... }`
  This line declares a **class** named `SchoolManagementSystem`. Think of a class as a blueprint or container for your program's logic. Because it's `public`, its name must match the file name.

- `public static void main(String[] args) { ... }`
  This is the **main method**. It's the special entry point for any Java application. When you run your program, the Java Virtual Machine (JVM) looks for this exact method signature and starts executing the code inside its curly braces `{}`.

- `System.out.println(...)`
  This is the command to **print** a line of text to the console. The `\n` within the text is a special "escape character" that tells the console to insert a newline, which is why the output appears on three separate lines.

---

## Compiling and Running the Program

This is the two-step process to bring your code to life.

#### 1. Compile the Code

We use the `javac` (Java Compiler) command to translate our human-readable `SchoolManagementSystem.java` file into machine-readable Java bytecode.

In your terminal (make sure you are still in the `SchoolManagementSystem` directory), run:

```bash
javac SchoolManagementSystem.java
```

If this command runs without any errors, you will see a new file appear in your directory: `SchoolManagementSystem.class`. This is the compiled bytecode.

> **Common Problem:** If you get an error like "class SchoolManagementSystem is public, should be declared in a file named SchoolManagementSystem.java", it means your file name does not exactly match the class name (check for typos and capitalization).

#### 2. Run the Program

Now, we use the `java` command to execute the compiled bytecode.

```bash
# Notice you do NOT include the .class extension
java SchoolManagementSystem
```

**Expected Output:** You should see the welcome message printed exactly as specified.

```
Welcome to CyberCrewz Academy
School Management System V1
Code name: River
```

---

## Pro-Engineer Checklist ✅

Let's establish professional habits from day one.

#### 1. Initialize Git Repository

This "turns on" version tracking for your project folder.

```bash
git init
```

You'll see a message like `Initialized empty Git repository in ...`.

#### 2. Create a `.gitignore` File

This file tells Git which files or folders to ignore. We don't want to track compiled files like `.class` because they are generated from source code.

Create a file named `.gitignore` and add the following line to it:

```
*.class
```

#### 3. Create a `README.md` File

The README file is the homepage of your project. It explains what the project is and how to use it.

Create a file named `README.md` and add some initial text:

```markdown
# School Management System (Code Name: River)

This is a project to re-learn Java fundamentals by building a console-based School Management System from scratch.

**Phase 1:** Procedural implementation with arrays and file handling.
```

#### 4. Make Your First Commit

A commit is a snapshot of your project at a specific point in time. We'll add all our new files and save them with a descriptive message.

```bash
# Stage all new and modified files for the commit
git add .

# Save the staged files with a message describing the change
git commit -m "Initial commit: Project setup and welcome screen"
```

Congratulations! You have successfully completed the first step. You have a working program and a version-controlled project foundation, ready for the next step: adding an interactive menu.
