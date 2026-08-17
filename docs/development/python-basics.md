# Python Basics

## 1. What is Python?

Python is a highly popular programming language for AI development. 

---

## 2. What do we need to start working with Python?

To write and run Python code on your computer, you need three things:

### I. Python Interpreter

This is the program that translates your Python code into instructions your computer’s processor can actually execute.

- **Where to get it:** Download **Python 3.14** from [python.org](https://www.python.org/downloads/).
- **What comes with it:** The interpreter includes a basic terminal runner and the package manager tool used to install external libraries called `pip`.

### II. Code Editor

It's the place where you write or generate code.

- **Recommended tools:** VS Code or Cursor.
- **Why use them?** They provide helpful features like suggesting code as you type, coloring code to make it readable, and debugging tools to spot errors.

### III. Libraries

Instead of writing everything from scratch, you can download code packages created by others. They are called libraries.

**Type in terminal:** 

```
pip install <library_name>
```

For example:

```
pip install fintom8
```

!!! tip
    To install all libraries needed for a project at once, we use a file called `requirements.txt`.

```
pip install -r requirements.txt
```

---

## 3. Do you need Venv?

Venv (Virtual Environment) is a dedicated Python environment only for a specific project, created in order not sharing it with other Python projects. This approach has pros and cons. If you work currently on your first Python project, you don't need it yet. An AI model might prompt you to create a virtual environment automatically. You can safely decline this step asking for utilization of the default Python environment.

---

## 4. Check Windows PATH

If you are on Windows, you need to give info to your Code Editor where to look for Python. For that you save the link to the Python file that starts Python (.exe file) to Windows PATH. When you type a command like `python` into your terminal in Code Editor next time, Windows quickly finds where the Python program lives and opens it for you.

**Without PATH:** You would have to type the giant, exact folder address every time you want to run a file:

```cmd
C:\Users\User\AppData\Local\Python\pythoncore-3.14-64\python.exe script.py
```

**With PATH:** Your computer already knows where to look. You only need to type in terminal:

```cmd
python script.py
```
!!! tip
    Ask AI model in Code Editor to save the link to the Python .exe file to Windows PATH. It will do it for you.
---

## 5. Using Python for files

In order to read the document or run any file in the project, you need to point Python to the right file locations.

### I. In Terminal

Type `python` followed by the full, exact address to the file:

```
python c:/Users/User/Documents/GitHub/f8_templates/fintom8/examples/invoice_extraction_fintom8.py
```

Or navigate to the folder first with `cd`, then type `python` followed by the filename:

```
cd c:/Users/User/Documents/GitHub/f8_templates/fintom8/examples
python invoice_extraction_fintom8.py
```
!!! tip
       You can use both `python` and `py` commands in terminal, but generally, prefer `python`.

### II. In Code Editor

If the document is in the same folder as your script, use `./` to represent "the current folder":

```
filePath = "./file.pdf"
```



