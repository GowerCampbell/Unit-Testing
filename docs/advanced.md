

# Learning Module: Advanced Python Testing Techniques

Welcome to the *Advanced Python Testing Techniques* module! This guide takes you beyond basic unit tests to master robust, scalable testing practices for building reliable Python applications.

## Advanced Testing Techniques

This section explores sophisticated testing strategies for handling complex scenarios, such as external services, asynchronous code, and varied input data.

### Key Concepts

- **Mocking**: Replace real objects (e.g., databases, APIs, file systems) with simulated ones to isolate tests. Python’s `unittest.mock` library is ideal for creating mocks.
- **Asynchronous Testing**: Use the `pytest-asyncio` plugin to test `async`/`await` code by managing the asyncio event loop effectively.
- **Parameterized Testing**: Reduce code duplication by running a single test function with multiple input sets using Pytest’s `@pytest.mark.parametrize`.
- **Fixtures**: Pytest fixtures provide reusable setup/teardown logic (e.g., database connections, temporary files) for consistent test environments.

### Example: Mocking a File-Reading Function

Here’s how to test a function that reads a file without touching the file system by mocking the `open` function.

```python
# function_to_test.py
def process_file_data(filepath):
    """Reads and processes file content."""
    with open(filepath, 'r') as f:
        content = f.read()
    return f"Processed: {content.strip()}"
```

```python
# test_functions.py
import unittest
from unittest.mock import patch, mock_open
from function_to_test import process_file_data

class TestProcessFileData(unittest.TestCase):
    def test_process_file_data_with_mock(self):
        """Test process_file_data by mocking file open."""
        mock_file_content = "some data"
        with patch('function_to_test.open', mock_open(read_data=mock_file_content)) as mocked_file:
            result = process_file_data('any/fake/path.txt')
            mocked_file.assert_called_once_with('any/fake/path.txt', 'r')
            self.assertEqual(result, "Processed: some data")
```

### Exercise: Mock an HTTP Request

1. Write a function that makes an HTTP GET request using the `requests` library and returns the response’s `status_code`.
2. Write a test using `unittest.mock.patch` to mock `requests.get`.
3. Assert the function returns the mocked status code and that `requests.get` was called with the correct URL.

## Testing Best Practices

Good tests are clear, maintainable, and reliable. Follow these practices to ensure your test suite is an asset, not a burden.

### Best Practices

- **Clear Test Names**: Use descriptive names like `test_add_positive_numbers` instead of vague ones like `test_add` to clarify intent.
- **Test Isolation**: Ensure tests are independent. Use `setUp`/`tearDown` in `unittest` or Pytest fixtures to reset state between tests.
- **Measure Coverage**: Use `coverage.py` to track which code is tested, identifying untested areas.
- **Refactor for Testability**: Improve code design with techniques like Dependency Injection to reduce tight coupling and global state.

### Example: Refactoring for Testability

**Original (Hard to Test)**: This function relies on a global config file.

```python
# Hard to test
CONFIG = {}

def load_config():
    global CONFIG
    with open("prod.cfg") as f:
        CONFIG['db_host'] = "prod.db.internal"

def get_database_host():
    if not CONFIG:
        load_config()
    return CONFIG.get('db_host')
```

**Refactored (Testable)**: Pass dependencies explicitly for easier testing.

```python
def get_database_host_from_config(config):
    """Get database host from a config dictionary."""
    return config.get('db_host')

# Test example
def test_get_database_host():
    mock_config = {'db_host': 'test.db.local'}
    host = get_database_host_from_config(mock_config)
    assert host == 'test.db.local'
```

### Exercise: Refactor and Test

Given this function reliant on a global variable:

```python
# global_state.py
USER_PERMISSIONS = {"admin": False}

def is_admin():
    """Checks if the current user is an admin."""
    return USER_PERMISSIONS["admin"]
```

1. Refactor `is_admin` to accept user data as an argument, removing reliance on `USER_PERMISSIONS`.
2. Write two tests: one verifying `True` for an admin user, another verifying `False` for a non-admin user.



This markdown preserves the structure and content of the original module while enhancing readability with clear headings, concise lists, and formatted code blocks. It includes all key sections, examples, and exercises as requested. Let me know if you need further adjustments or additional details!
