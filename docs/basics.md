
# Unit Testing & Modules in Python

## Why Unit Testing?
- We can test progroms to ensure they run correctly and prevents bugs from syntax, logic, compiltion, runtime, arithmetic, resource, interface linker and semantic errors
- By catching errors early in development, we can spend more time maintaining and improve our codes, especially when refactor existing source code nd restucturing without maintaining its external behaviour.

## AAA Pattern (Arrange-Act-Assert)
- **Arrange**: Set up the test data and environment using the code we have written.
- **Act**: Execute the function or method being tested and see how it uses variables inputted
- **Assert**: Verify the expected outcome and see if it can be broken.

## FIRST Principles in Testing
- **Fast**: Tests should execute quickly and not waste resources forming an output from the algorithms time complexity.
- **Independent**: Tests should not rely on each other, and take up more memory than needed out of space complexity .
- **Repeatable**: Tests should yield consistent nd repeatable results.
- **Self-Validating**: Tests should clearly pass or fail when checked.
- **Thorough**: Tests should cover various scenarios and edge cases in the codes uses.

## Better Code Organization with Modules
- Split large codebases into reusable modules, ensuring tests are relevant to specfic features.
- Enhances maintainability for future updates, allowing exchanges with new configerations.
- Facilitates collaboration by reducing tight coupling (degree of interdepence between software modules and how reliant they are on different scripts from other modules.
- Encapsulates functionality for better isolation, especially with cohersion (degree to which elements inside a module or script belong together.

## Example: Creating and Importing a Module
```python
# my_module.py
def greet(name: str) -> str:
    """Returns a greeting message."""
    return f"Hello, {name}!"

# main.py
if __name__ == "__main__":
    import my_module
    print(my_module.greet("Alice"))
```

## Virtual Environments (venv)
- Isolates project-specific dependencies, packaging them for the Virtual machines.
- Prevents version conflicts between packages and applicatios.

### Commands for Virtual Environments (Reminder)
```bash
# Create a virtual environment
python -m venv myenv

# Activate the virtual environment
# Windows
myenv\Scripts\activate
# macOS/Linux
source myenv/bin/activate

# Deactivate the virtual environment
deactivate

# Install a package
pip install package_name

# Export dependencies to a file
pip freeze > requirements.txt

# Install dependencies from a file
pip install -r requirements.txt
```

## PEP 8 & PEP 484 (Coding Standards)
- **PEP 8**: Style guide for readable Python code, ensure you arent using extra space and making the proper indentation.
- **PEP 484**: Introduces type hinting for better code clarity and annotations that allow you to write syntax for different types of code.

```python
def add(num1: int, num2: int) -> int:
    """Return the sum of two integers."""
    return num1 + num2
```

### Linting with pylint
```bash
pylint my_script.py
```

## Unit Testing in Python
```python
import unittest

def divide(a: float, b: float) -> float:
    """Returns the quotient of a and b, raises ZeroDivisionError if b is zero."""
    return a / b

class TestMathOperations(unittest.TestCase):
    def test_divide(self):
        """Test divide function for ZeroDivisionError."""
        with self.assertRaises(ZeroDivisionError):
            divide(5, 0)

if __name__ == "__main__":
    unittest.main()
```

## Common Causes of Test Failures
- Incorrect assertions (True or False)
- Unexpected function behavior
      (incorrect varaible scope; function calling variables that are not handled)
      (Mutable Default Arguments; Mutable  objects e.g lists, dictionaries that over accumulte items)
      (Floating Point Precision; Approximations and small errors due to limitations using float values)
- Environment or dependency issues.(logical, assumptions or environmental factors)

## Debugging Failing Tests
- Verify assertions from the test coverge look for edge cases(exceptions in your input handling)
- Inspect function logic for errors (Find Discrepncy between generated code and test expectations)
- Ensure dependencies are properly configured and using varaiables that are initialised in a test setup.
- Using 'mocks' to affirm interactions and behviours and 'stubs' to provide predefined responses using an actor.

## Unit Testing & Test-Driven Development (TDD)
*Written by Gower Campbell*

Unit testing validates individual code components (e.g., functions, classes, or modules) to ensure they work as intended. TDD involves writing tests before the production code, guiding development and ensuring that the desired functionlity then writing the minimum code necessry.

### What is Unit Testing?
Unit testing tests individual components in isolation to catch defects early. Python’s `unittest` framework is commonly used for this purpose.

### What is a Failing Test?
A failing test does not produce the expected outcome, indicating a bug or discrepancy. In TDD, failing tests are written first as specifications that the code must satisfy. Setting Automtions of what we want the actuasl code to do.

### The Iterative Process of TDD
TDD follows an iterative cycle:
1. Red. Write a failing test.
2. Green. Write the minimum code to pass the test.
3. Refactor. Refactor the code while ensuring tests still pass to remove duplicate/redundancys in the code.
3. Repeat. Repeat for each new feature or change.

### Unit Test Life Cycle
1. Check out the code repository.
2. Make code changes.
3. Run unit tests.
4. Fix defects and rerun tests.
5. Conduct a code review.
6. Check the code into the repository.

## Example: TodoList Class and Unit Tests
```python
# todo_list.py
class TodoList:
    def __init__(self):
        self.tasks = []

    def add_task(self, task):
        self.tasks.append(task)

    def update_task(self, old_task, new_task):
        if old_task in self.tasks:
            index = self.tasks.index(old_task)
            self.tasks[index] = new_task

    def remove_task(self, task):
        if task in self.tasks:
            self.tasks.remove(task)

# test_todo_list.py
import unittest
from todo_list import TodoList

class TestTodoList(unittest.TestCase):
    def setUp(self):
        self.todo_list = TodoList()

    def test_add_task(self):
        self.todo_list.add_task("Task 1")
        self.assertIn("Task 1", self.todo_list.tasks)

    def test_update_task(self):
        self.todo_list.add_task("Task 1")
        self.todo_list.update_task("Task 1", "Updated Task 1")
        self.assertIn("Updated Task 1", self.todo_list.tasks)
        self.assertNotIn("Task 1", self.todo_list.tasks)

    def test_remove_task(self):
        self.todo_list.add_task("Task 1")
        self.todo_list.remove_task("Task 1")
        self.assertNotIn("Task 1", self.todo_list.tasks)

if __name__ == '__main__':
    unittest.main()
```

### Running the Tests
```python
# test_suite.py
import unittest
from test_todo_list import TestTodoList

suite = unittest.TestLoader().loadTestsFromTestCase(TestTodoList)
unittest.TextTestRunner().run(suite)
```

### How to Run Tests
1. Navigate to the project directory:
   ```bash
   cd /path/to/project/
   ```
2. Run the test suite:
   ```bash
   python test_suite.py
   ```
3. Check the terminal for results. A failing test shows `F` or `FAIL`, while a passing test shows `OK`.

### Example Output
```
F..
======================================================================
FAIL: test_add_task (test_todo_list.TestTodoList.test_add_task)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "path\to\file\test_todo_list.py", line 13, in test_add_task
    self.assertEqual(self.todo_list.tasks, [])
AssertionError: Lists differ: ['Task 1'] != []
First list contains 1 additional elements.
First extra element 0:
'Task 1'
- ['Task 1']
+ []
----------------------------------------------------------------------
Ran 3 tests in 0.001s
FAILED (failures=1)
```

## Best Practices
- **Separation of Files**: Keep implementation and test files separate (e.g., `todo_list.py` and `test_todo_list.py`).
- **Related Test Cases**: Group related tests in the same class, with each method as an independent test case.
- **Clear Assertions**: Use descriptive assertions for clarity following coding standrd.
- **Timely Testing**: Write tests before or alongside implementation code to ensure they work.

## Example: Calculator Class and Unit Tests
```python
# calculator.py
class ErrorDivisionZero(Exception):
    """Custom exception for handling division by zero."""
    pass

class Calculator:
    """A simple calculator class with methods to add, subtract, multiply, and divide."""
    
    def add(self, a, b):
        """Returns the sum of a and b."""
        return a + b
    
    def subtract(self, a, b):
        """Returns the difference of a and b."""
        return a - b
    
    def multiply(self, a, b):
        """Returns the product of a and b."""
        return a * b
    
    def divide(self, a, b):
        """Returns the quotient of a and b. Raises ErrorDivisionZero if dividing by zero."""
        if b == 0:
            raise ErrorDivisionZero("Cannot divide by zero")
        return a / b

# test_calculator.py
import unittest
from calculator import Calculator, ErrorDivisionZero

class TestCalculator(unittest.TestCase):
    def setUp(self):
        """Create an instance of the Calculator class to be used in each test."""
        self.calculator = Calculator()

    def test_add(self):
        """Test addition with both positive and negative numbers."""
        self.assertEqual(self.calculator.add(3, 5), 8)
        self.assertEqual(self.calculator.add(-3, 5), 2)
        self.assertEqual(self.calculator.add(-3, -5), -8)

    def test_subtract(self):
        """Test subtraction with both positive and negative numbers."""
        self.assertEqual(self.calculator.subtract(10, 5), 5)
        self.assertEqual(self.calculator.subtract(-3, 5), -8)
        self.assertEqual(self.calculator.subtract(-5, -3), -2)

    def test_multiply(self):
        """Test multiplication with both positive and negative numbers."""
        self.assertEqual(self.calculator.multiply(3, 5), 15)
        self.assertEqual(self.calculator.multiply(-3, 5), -15)
        self.assertEqual(self.calculator.multiply(-3, -5), 15)

    def test_divide(self):
        """Test division with both positive and negative numbers, and handle division by zero."""
        self.assertEqual(self.calculator.divide(10, 2), 5)
        self.assertEqual(self.calculator.divide(-10, 2), -5)
        self.assertEqual(self.calculator.divide(-10, -2), 5)
        with self.assertRaises(ErrorDivisionZero):
            self.calculator.divide(10, 0)

    def test_error_divide_by_zero(self):
        """Test that division by zero raises the appropriate custom exception."""
        with self.assertRaises(ErrorDivisionZero):
            self.calculator.divide(1, 0)

if __name__ == '__main__':
    unittest.main()
```

## Example: Library System and Unit Tests
```python
# library.py
class Library:
    def __init__(self):
        self.books = []

    def add_book(self, book):
        """Add a book to the library."""
        self.books.append(book)

    def remove_book(self, book):
        """Remove a book from the library if it exists."""
        if book in self.books:
            self.books.remove(book)

    def find_book(self, book):
        """Return True if the book is in the library, False otherwise."""
        return book in self.books

# test_library.py
import unittest
from library import Library

class TestLibrary(unittest.TestCase):
    def setUp(self):
        """Create a new Library instance before each test."""
        self.library = Library()

    def test_add_book(self):
        """Test adding a book to the library."""
        self.library.add_book("Python Programming")
        self.assertIn("Python Programming", self.library.books)

    def test_remove_book(self):
        """Test removing a book from the library."""
        self.library.add_book("Python Programming")
        self.library.remove_book("Python Programming")
        self.assertNotIn("Python Programming", self.library.books)

    def test_find_book(self):
        """Test finding a book in the library."""
        self.library.add_book("Python Programming")
        self.assertTrue(self.library.find_book("Python Programming"))
        self.assertFalse(self.library.find_book("Java Programming"))

if __name__ == '__main__':
    unittest.main()
```

## Conclusion
Unit testing and TDD enhance code quality, reduce bugs, and support agile development. By adhering to the AAA pattern, FIRST principles, and best practices like modular code and timely testing, developers can create reliable, maintainable software.

