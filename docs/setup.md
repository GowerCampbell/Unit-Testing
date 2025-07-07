# Setting Up Your Python Testing Environment

A well-configured testing environment is the foundation of effective Python testing. This module guides you through setting up a clean, isolated, and organized development environment to write and run tests efficiently.

## 1. Setting Up the Environment

This section outlines the essential steps to prepare your system and project for Python testing.

### Key Steps

- **Install Python 3.8+**: Use a modern Python version for compatibility with testing tools. Download it from the [official Python website](https://www.python.org/downloads/).
- **Create a Virtual Environment**: Isolate project dependencies using Python’s `venv` module to prevent package conflicts.
  - Create: `python -m venv myenv`
  - Activate (Windows): `myenv\Scripts\activate`
  - Activate (macOS/Linux): `source myenv/bin/activate`
- **Install Testing Frameworks**:
  - **pytest**: A feature-rich testing framework. Install with `pip install pytest`.
  - **unittest**: Built into Python’s standard library, requiring no installation.
- **Organize Project Structure**: Keep code and tests separate for maintainability. Recommended layout:
  ```
  my_project/
  ├── src/
  │   ├── __init__.py
  │   └── module.py
  └── tests/
      ├── __init__.py
      └── test_module.py
  ```

### Example: Setting Up a Virtual Environment with pytest

Follow these terminal commands to initialize your testing environment:

```bash
# Create project directory
mkdir my_project
cd my_project

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate

# Install pytest
pip install pytest
```

After activation, your terminal prompt should show `(venv)`. The `pip install pytest` command will confirm successful installation.

### Exercise: Set Up a Project and Run a Sample Test

1. **Create the Project**:
   - Follow the example above to create a `my_project` directory and set up a virtual environment with `pytest` installed.
   - Create the `src/` and `tests/` folders as shown in the recommended structure.

2. **Add a Simple Function**:
   - Create `src/calculator.py` with the following code:

```python
# src/calculator.py
def add(a, b):
    """Add two numbers."""
    return a + b
```

3. **Write a Test**:
   - Create `tests/test_calculator.py` with the following test:

```python
# tests/test_calculator.py
from src.calculator import add

def test_add():
    """Test the add function with positive and negative numbers."""
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
```

4. **Run the Test**:
   - From the `my_project` directory, run:
     ```bash
     pytest
     ```
   - You should see output indicating that the tests passed (e.g., `2 passed`).

### Tips for Success

- Verify Python version with `python --version` before starting.
- Ensure the virtual environment is activated before running `pip` or `pytest`.
- If tests don’t run, check that `tests/` contains an `__init__.py` file and that file paths in imports are correct.
- Use `pytest -v` for verbose output to see detailed test results.

