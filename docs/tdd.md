# An In-Depth Guide to Test-Driven Development (TDD)

Test-Driven Development (TDD) is a software development process that flips the traditional model on its head. Instead of writing my application code and then writing tests for it, TDD requires you to write a failing test *before* you write the production code. This simple switch has a profound impact on code quality, design, and developer confidence.

This guide will walk you through the TDD cycle, its benefits, and practical examples to get you started.

-----

## The TDD Cycle: Red, Green, Refactor

The core of TDD is a short, repetitive cycle. Each small piece of new functionality begins with a new cycle.

### 🔴 Step 1: Red — Write a Failing Test

First, you write an automated test for a new feature or a small piece of functionality. Since you haven't written any implementation code yet, this test is expected to fail. The goal is to clearly define what you want the code to do and to see the test fail for the right reasons. This confirms your test runner is working correctly.

### 🟢 Step 2: Green — Write Code to Make the Test Pass

Next, you write the **simplest possible code** to get the test to pass. The focus here is not on writing elegant or perfect code, but on fulfilling the requirements of the test. This step ensures that you are only writing code in response to a specific, tested requirement.

### 🔵 Step 3: Refactor — Improve the Code

With a passing test as your safety net, you can now clean up the code you just wrote. Refactoring involves improving the code's structure, removing duplication, and increasing clarity without changing its external behavior. Because you have a test that passes, you can be confident that your improvements haven't broken the functionality.

This cycle is repeated for every new piece of functionality, creating a robust, well-tested, and well-designed codebase.

-----

## Core Benefits of TDD

Adopting TDD offers significant advantages that lead to better software.

  * **Fewer Bugs**: By writing tests for every piece of functionality, my create a comprehensive regression suite. This test suite acts as a safety net, allowing you to make changes with confidence and catch bugs before they reach production.
  * **Better Design**: TDD forces you to think about the code from the perspective of its user. This "test-first" approach leads to more modular, decoupled, and maintainable code with cleaner interfaces. my only write code that is necessary to pass a test, avoiding over-engineering.
  * **Clear Requirements**: Each test acts as an executable specification for a small piece of the system. It removes ambiguity from requirements and provides living documentation of how the system is supposed to behave.

-----

## Example: Implementing a `TodoList` using TDD

Let's walk through implementing a `TodoList` class using the Red-Green-Refactor cycle.

### Red: Write a Failing Test for `add_task`

We start by defining what it means to add a task. A task added to the list should be present in the list.

**`tests/test_todo_list.py`**

```python
import unittest
from todo_list import TodoList

class TestTodoList(unittest.TestCase):
    def test_add_task(self):
        # We need a TodoList class first, so this will fail.
        todo_list = TodoList()
        todo_list.add_task("Buy groceries")
        self.assertIn("Buy groceries", todo_list.tasks)
```

Running this test will result in an error because the `TodoList` class doesn't exist. This is our **Red** state.

### Green: Make the Test Pass

Now, I write the simplest code possible to make the test pass.

**`todo_list.py`**

```python
class TodoList:
    def __init__(self):
        self.tasks = []

    def add_task(self, task):
        self.tasks.append(task)
```

Now, if we run our test, it will pass. I have a `TodoList` class with an `add_task` method that works as expected. This is my **Green** state.

### Refactor: Improve the Code

In this case, the code is very simple and doesn't need much improvement. The names are clear, and there's no duplication. The refactoring step here is simply a quick review to confirm the code's quality.

Let's extend the example to see refactoring in action. Let's add a `complete_task` feature.

**Red: Test for completing a task**

```python
# In tests/test_todo_list.py
# ...
    def test_complete_task(self):
        self.todo_list.add_task("Do laundry")
        self.todo_list.complete_task("Do laundry")
        self.assertNotIn("Do laundry", self.todo_list.tasks)
        self.assertIn("Do laundry", self.todo_list.completed_tasks)
```

This test will fail.

**Green: Make it pass**

```python
# In todo_list.py
class TodoList:
    def __init__(self):
        self.tasks = []
        self.completed_tasks = [] # Add a new list

    def add_task(self, task):
        self.tasks.append(task)

    def complete_task(self, task):
        self.tasks.remove(task)
        self.completed_tasks.append(task)
```

The test now passes.

**Refactor: Improve the implementation**
Our `TodoList` class is getting more complex. I could refactor the internal data structure to be more robust, perhaps using a dictionary to store task status instead of two separate lists.

```python
# In todo_list.py (Refactored)
class TodoList:
    def __init__(self):
        # Use a dictionary to store tasks and their status
        self._tasks = {}

    def add_task(self, task):
        self._tasks[task] = 'incomplete'

    def complete_task(self, task):
        if task in self._tasks:
            self._tasks[task] = 'complete'

    @property
    def tasks(self):
        return [task for task, status in self._tasks.items() if status == 'incomplete']

    @property
    def completed_tasks(self):
        return [task for task, status in self._tasks.items() if status == 'complete']
```

After this refactoring, we re-run our tests. They still pass, but our internal implementation is now cleaner and more scalable.

-----

## 🏋️ HyperionDev Exercise: Implement a `Calculator` using TDD

Now it's my turn. Implement a simple `Calculator` class that can perform addition, subtraction, multiplication, and division. Follow the TDD cycle strictly.

### 1\. Project Setup

Create two files:

  * `calculator.py` (this will be empty initially)
  * `tests/test_calculator.py`

### 2\. The `add` Method (Red-Green-Refactor)

**🔴 Red:** Write a failing test for the `add` method in `tests/test_calculator.py`.

```python
# tests/test_calculator.py
import unittest
from calculator import Calculator

class TestCalculator(unittest.TestCase):
    def setUp(self):
        self.calc = Calculator()

    def test_add(self):
        self.assertEqual(self.calc.add(2, 3), 5)
        self.assertEqual(self.calc.add(-1, 1), 0)
```

**🟢 Green:** Write the minimum code in `calculator.py` to make this test pass.

**🔵 Refactor:** Review your `add` method and test. Is it clean? Can it be improved?

### 3\. The `subtract` Method (Red-Green-Refactor)

**🔴 Red:** Write a new test for a `subtract` method.

**🟢 Green:** Implement the `subtract` method to make the test pass.

**🔵 Refactor:** Clean up the code.

### 4\. Continue the Cycle

Repeat the TDD cycle for the following functionalities:

  * **Multiplication (`multiply`)**
  * **Division (`divide`)**
  * **Bonus**: Add a test to ensure that dividing by zero raises an error (e.g., `ValueError`). Use `self.assertRaises` in your test case.

By the end of this exercise, you will have a fully tested `Calculator` class and a deeper understanding of the Test-Driven Development workflow.
