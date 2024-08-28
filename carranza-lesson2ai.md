<!-- 

Austin Carranza
27 August 2024
CSC-256 Lesson 2
NOTE: Using ChatGPT for this lesson 

-->


# Inital Prompt:
I want to create a calculator program in python using the procedural paradigm. Initially, the feature will be for addition, subtraction, multiplication, and division. I will add other features such as present value, mortgage payments, etc. later so the design must easily allow this.

# AI Feedback from inital prompt:
Here's a basic structure for your calculator program that follows the procedural paradigm. This structure will allow you to easily add more features later.

### 1. **Define Functions for Basic Operations:**

```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b != 0:
        return a / b
    else:
        return "Error: Division by zero"
```

### 2. **Create a Main Function to Handle User Input:**

This function will allow the user to select which operation to perform.

```python
def main():
    print("Select operation:")
    print("1. Addition")
    print("2. Subtraction")
    print("3. Multiplication")
    print("4. Division")

    choice = input("Enter choice (1/2/3/4): ")

    if choice in ['1', '2', '3', '4']:
        num1 = float(input("Enter first number: "))
        num2 = float(input("Enter second number: "))

        if choice == '1':
            print(f"Result: {add(num1, num2)}")
        elif choice == '2':
            print(f"Result: {subtract(num1, num2)}")
        elif choice == '3':
            print(f"Result: {multiply(num1, num2)}")
        elif choice == '4':
            print(f"Result: {divide(num1, num2)}")
    else:
        print("Invalid input")

if __name__ == "__main__":
    main()
```

### 3. **Adding More Features Later:**

To add more features later, simply define additional functions (e.g., `def present_value(...):`) and update the `main()` function to include these new operations. The design ensures that expanding functionality is straightforward by adding more options and corresponding function calls.

### 4. **Example of Adding Present Value Calculation:**

```python
def present_value(future_value, rate, periods):
    return future_value / (1 + rate) ** periods

# Update the main function to include the new feature
def main():
    print("Select operation:")
    print("1. Addition")
    print("2. Subtraction")
    print("3. Multiplication")
    print("4. Division")
    print("5. Present Value Calculation")

    choice = input("Enter choice (1/2/3/4/5): ")

    if choice in ['1', '2', '3', '4']:
        num1 = float(input("Enter first number: "))
        num2 = float(input("Enter second number: "))

        if choice == '1':
            print(f"Result: {add(num1, num2)}")
        elif choice == '2':
            print(f"Result: {subtract(num1, num2)}")
        elif choice == '3':
            print(f"Result: {multiply(num1, num2)}")
        elif choice == '4':
            print(f"Result: {divide(num1, num2)}")
    elif choice == '5':
        fv = float(input("Enter future value: "))
        rate = float(input("Enter rate (as a decimal): "))
        periods = int(input("Enter number of periods: "))
        print(f"Present Value: {present_value(fv, rate, periods)}")
    else:
        print("Invalid input")

if __name__ == "__main__":
    main()
```

This approach keeps your code modular and easy to extend as you add more complex financial calculations.

# Second Prompt (Unit Testing):
    Can unit tests using `Pytest` be created to test the code easily with minimum mocking?

# AI Feedback from Second Prompt (Unit Testing):
Yes, you can create unit tests for this calculator program using `Pytest` without much need for mocking. Since the functions are simple and don’t have external dependencies, you can directly test their outputs given specific inputs.

Here’s an example of how you can write unit tests for the basic arithmetic functions:

### 1. **Install Pytest (if not already installed):**

You can install `Pytest` using pip:

```bash
pip install pytest
```

### 2. **Create a Test File:**

Create a file named `test_calculator.py` in the same directory as your calculator code.

### 3. **Write Unit Tests:**

In `test_calculator.py`, you can write the following tests:

```python
import pytest
from calculator import add, subtract, multiply, divide, present_value

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

def test_subtract():
    assert subtract(10, 5) == 5
    assert subtract(-1, -1) == 0
    assert subtract(0, 0) == 0

def test_multiply():
    assert multiply(3, 4) == 12
    assert multiply(-2, 2) == -4
    assert multiply(0, 100) == 0

def test_divide():
    assert divide(10, 2) == 5
    assert divide(-10, -2) == 5
    assert divide(0, 1) == 0
    assert divide(5, 0) == "Error: Division by zero"

def test_present_value():
    assert present_value(100, 0.05, 1) == pytest.approx(95.238, 0.001)
    assert present_value(1000, 0.05, 10) == pytest.approx(613.913, 0.001)
    assert present_value(1000, 0, 10) == 1000
```

### 4. **Run the Tests:**

You can run the tests using the following command:

```bash
pytest test_calculator.py
```

### 5. **Explanation:**

- **Basic Arithmetic Tests (`test_add`, `test_subtract`, `test_multiply`, `test_divide`):** These tests check that the functions return the expected values for various inputs.
- **Division by Zero (`test_divide`):** This test checks the behavior when dividing by zero, ensuring the function handles this edge case correctly.
- **Present Value Calculation (`test_present_value`):** This test checks that the `present_value` function returns a value close to the expected result. The `pytest.approx` function allows for comparison with a small tolerance for floating-point precision.

### 6. **Adding More Tests:**

As you add more features to your calculator, you can similarly add more tests to this file. The structure allows you to test each function independently, which is ideal for unit testing.

This setup requires minimal mocking because the functions operate on inputs directly and do not depend on external systems. This makes it easy to test and maintain.

# Third Prompt (Refactoring Prompt):
Does the code meet the following principles? If not, where and how do I refactor? Code that meets SOLID principles as well as DRY, KISS, YAGNI, Law of Demeter, Composition over inheritance, Encapsulation, High Cohesion, Low Coupling, modular design, OOP principles. Code should be easy to maintain, easy to add features, and easy to test using `Pytest`.

# AI Feedback from Third Prompt (Refactoring Prompt):
The code provided follows some good design principles but can be improved to better adhere to SOLID principles, DRY, KISS, YAGNI, and other guidelines you've mentioned. To align it more closely with these principles, a refactor is necessary. Below, I’ll break down how the code can be refactored to meet these principles:

### 1. **Single Responsibility Principle (SRP):**
   - **Issue:** The `main()` function does too much: it handles user input, performs calculations, and displays results.
   - **Solution:** Separate concerns by creating distinct functions for input handling, operation selection, and output display.

### 2. **Open/Closed Principle (OCP):**
   - **Issue:** The current design requires modifying the `main()` function whenever a new feature is added.
   - **Solution:** Use a dictionary to map operations to functions. This allows new operations to be added without modifying existing code.

### 3. **Liskov Substitution Principle (LSP):**
   - **No issues:** Basic functions like `add`, `subtract`, etc., don't violate LSP as they are simple operations.

### 4. **Interface Segregation Principle (ISP):**
   - **No issues:** Since we don't have interfaces in this simple code, ISP is not directly applicable.

### 5. **Dependency Inversion Principle (DIP):**
   - **Issue:** The `main()` function depends on concrete functions for operations, which isn't ideal for extensibility.
   - **Solution:** You can abstract the operations into a class or module, though this is less critical in a small procedural script.

### 6. **DRY (Don't Repeat Yourself):**
   - **Issue:** The code that gets user input and selects operations is repeated for each operation.
   - **Solution:** Refactor common logic into helper functions.

### 7. **KISS (Keep It Simple, Stupid):**
   - **No issues:** The code is relatively simple but can be further simplified with modularization.

### 8. **YAGNI (You Aren't Gonna Need It):**
   - **No issues:** The current design avoids unnecessary features.

### 9. **Law of Demeter:**
   - **No issues:** The current design is simple enough that it doesn't violate this law.

### 10. **Composition Over Inheritance:**
   - **No issues:** The code doesn’t use inheritance. If moved to OOP, use composition for reusability.

### 11. **Encapsulation:**
   - **Issue:** The procedural design doesn't encapsulate data or behavior.
   - **Solution:** Consider moving to an OOP design if the project scales.

### 12. **High Cohesion, Low Coupling:**
   - **Issue:** The `main()` function has low cohesion (does multiple things).
   - **Solution:** Split responsibilities into different functions or classes.

### 13. **Modular Design:**
   - **Issue:** The code isn't fully modular, especially in terms of adding new features.
   - **Solution:** Refactor into a more modular structure.

### **Refactored Code:**

```python
# operations.py - Module for mathematical operations

def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b != 0:
        return a / b
    else:
        return "Error: Division by zero"

def present_value(future_value, rate, periods):
    return future_value / (1 + rate) ** periods

# calculator.py - Main calculator module

import operations

def get_user_input():
    num1 = float(input("Enter first number: "))
    num2 = float(input("Enter second number: "))
    return num1, num2

def perform_operation(operation_func, num1, num2):
    return operation_func(num1, num2)

def display_result(result):
    print(f"Result: {result}")

def main():
    operations_map = {
        '1': operations.add,
        '2': operations.subtract,
        '3': operations.multiply,
        '4': operations.divide,
        '5': operations.present_value
    }

    print("Select operation:")
    print("1. Addition")
    print("2. Subtraction")
    print("3. Multiplication")
    print("4. Division")
    print("5. Present Value Calculation")

    choice = input("Enter choice (1/2/3/4/5): ")

    if choice in operations_map:
        if choice == '5':
            fv = float(input("Enter future value: "))
            rate = float(input("Enter rate (as a decimal): "))
            periods = int(input("Enter number of periods: "))
            result = operations_map[choice](fv, rate, periods)
        else:
            num1, num2 = get_user_input()
            result = perform_operation(operations_map[choice], num1, num2)
        
        display_result(result)
    else:
        print("Invalid input")

if __name__ == "__main__":
    main()
```

### **Explanation of Refactoring:**

1. **Modularity:** The operations are placed in a separate module (`operations.py`), improving modularity. This also adheres to SRP and High Cohesion by keeping related functionality together.
   
2. **Open/Closed Principle:** The `operations_map` dictionary in the `main()` function allows the program to be extended with new operations without modifying existing code.

3. **Encapsulation:** The logic for handling user input, performing operations, and displaying results is encapsulated in distinct functions, making the code easier to test and maintain.

4. **KISS and DRY:** The code is simple and avoids repetition by refactoring common logic into functions.

5. **Low Coupling:** The `main()` function is loosely coupled to the operations, relying on abstracted functions rather than direct implementations.

### **Testing with Pytest:**

The refactoring does not affect the ability to write tests using `Pytest`. The same test structure as mentioned earlier applies, with the addition that you may need to adjust imports in the test file based on the new structure.

### **Conclusion:**

This refactored design adheres more closely to the principles of SOLID, DRY, KISS, and other guidelines, making it easier to maintain, extend, and test the code.



# Fourth Prompt (Feature Addition and Modularization Prompt):
Add a new feature that would require a database or JSON file. Modularize the code into at least four modules to help in team development, testing, maintenance, and minimizing refactoring while adding features.

# AI Feedback from Fourth Prompt (Feature Addition and Modularization Prompt):
Let's add a new feature to the calculator: saving and loading previous calculations. This feature will require storing the results in a persistent format, such as a JSON file. We'll modularize the code into four modules: `operations`, `io_handler`, `calculator`, and `storage`. This will help in team development, testing, and maintenance.

### **1. Modules Overview:**

1. **`operations.py`:** Contains all mathematical operations (addition, subtraction, etc.).
2. **`io_handler.py`:** Handles user input and output, making it easier to modify the user interface.
3. **`storage.py`:** Manages saving and loading calculations from a JSON file, encapsulating all file operations.
4. **`calculator.py`:** The main module that ties everything together, handling the main program logic.

### **2. Module: `operations.py`**

This module will contain the basic arithmetic operations and any future financial functions.

```python
# operations.py

def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b != 0:
        return a / b
    else:
        return "Error: Division by zero"

def present_value(future_value, rate, periods):
    return future_value / (1 + rate) ** periods
```

### **3. Module: `io_handler.py`**

This module will handle all user interactions.

```python
# io_handler.py

def get_user_input():
    num1 = float(input("Enter first number: "))
    num2 = float(input("Enter second number: "))
    return num1, num2

def get_operation_choice():
    print("Select operation:")
    print("1. Addition")
    print("2. Subtraction")
    print("3. Multiplication")
    print("4. Division")
    print("5. Present Value Calculation")
    print("6. Save Calculation")
    print("7. Load Calculations")

    return input("Enter choice (1/2/3/4/5/6/7): ")

def display_result(result):
    print(f"Result: {result}")

def display_calculations(calculations):
    if calculations:
        print("Previous Calculations:")
        for calc in calculations:
            print(calc)
    else:
        print("No previous calculations found.")
```

### **4. Module: `storage.py`**

This module will manage storing and loading calculations from a JSON file.

```python
# storage.py

import json
import os

FILE_PATH = 'calculations.json'

def save_calculation(calculation):
    data = load_calculations()
    data.append(calculation)
    with open(FILE_PATH, 'w') as f:
        json.dump(data, f)

def load_calculations():
    if not os.path.exists(FILE_PATH):
        return []
    with open(FILE_PATH, 'r') as f:
        return json.load(f)
```

### **5. Module: `calculator.py`**

This module is the main entry point and ties all the other modules together.

```python
# calculator.py

import operations
import io_handler
import storage

def main():
    operations_map = {
        '1': operations.add,
        '2': operations.subtract,
        '3': operations.multiply,
        '4': operations.divide,
        '5': operations.present_value
    }

    while True:
        choice = io_handler.get_operation_choice()

        if choice in operations_map:
            if choice == '5':
                fv = float(input("Enter future value: "))
                rate = float(input("Enter rate (as a decimal): "))
                periods = int(input("Enter number of periods: "))
                result = operations_map[choice](fv, rate, periods)
            else:
                num1, num2 = io_handler.get_user_input()
                result = operations_map[choice](num1, num2)

            io_handler.display_result(result)

        elif choice == '6':  # Save Calculation
            num1, num2 = io_handler.get_user_input()
            operation_choice = io_handler.get_operation_choice()
            result = operations_map[operation_choice](num1, num2)
            storage.save_calculation({
                "operation": operation_choice,
                "num1": num1,
                "num2": num2,
                "result": result
            })
            io_handler.display_result("Calculation saved.")

        elif choice == '7':  # Load Calculations
            calculations = storage.load_calculations()
            io_handler.display_calculations(calculations)

        else:
            print("Invalid input")

if __name__ == "__main__":
    main()
```

### **6. Explanation of the Refactor:**

1. **Modularity:** 
   - Code is split into distinct modules with clear responsibilities:
     - `operations.py` handles all calculations.
     - `io_handler.py` manages user input/output.
     - `storage.py` manages data persistence.
     - `calculator.py` coordinates everything.
   
2. **Encapsulation:**
   - Each module encapsulates its own functionality, promoting high cohesion and low coupling.
   - This design ensures changes in one module (e.g., changing from JSON to a database) don’t affect others.

3. **Extensibility:**
   - New features can be added with minimal changes. For instance, adding a new operation requires only an additional function in `operations.py` and an entry in `calculator.py`.
   - New storage mechanisms can be added by modifying or extending `storage.py` without altering the main logic.

4. **Ease of Testing:**
   - Each module can be tested independently using `Pytest`. For example, `operations.py` can be tested for correct calculations, `storage.py` for correct file handling, and `io_handler.py` for correct interaction logic.
   - Mocking can be used in tests to simulate user input/output and file operations.

### **7. Testing with Pytest:**

You can create a separate test file for each module, e.g., `test_operations.py`, `test_storage.py`, etc. Here's an example test for the `storage.py` module:

```python
# test_storage.py

import storage
import os
import json

def test_save_load_calculation():
    test_data = {
        "operation": "1",
        "num1": 10,
        "num2": 5,
        "result": 15
    }
    
    # Ensure the file doesn't exist before the test
    if os.path.exists(storage.FILE_PATH):
        os.remove(storage.FILE_PATH)
    
    storage.save_calculation(test_data)
    loaded_data = storage.load_calculations()
    
    assert len(loaded_data) == 1
    assert loaded_data[0] == test_data

    # Clean up after test
    os.remove(storage.FILE_PATH)
```

This test ensures that calculations are correctly saved and loaded from the JSON file.

### **Conclusion:**

This refactoring meets the principles of SOLID, DRY, KISS, and modular design, making the code more maintainable, extensible, and testable. Each module has a clear responsibility, allowing different team members to work on different parts of the codebase independently.



# Fifth Prompt (Web Application Refactoring Prompt):
If I wanted to eventually turn this into a web application, what refactoring should I do to allow this in the future?

# AI Feedback from Fifth Prompt (Web Application Refactoring Prompt):
To eventually turn this calculator program into a web application, you can refactor the code to make the transition smoother. Here’s what you should consider:

### 1. **Abstract Input/Output Handling:**
   - **Why:** Web applications have different input/output mechanisms (HTTP requests/responses) compared to console applications.
   - **How:** Decouple the I/O logic from the core functionality so that it can be easily swapped out or extended.
   
### **Refactoring Steps:**

#### **Step 1: Separate Business Logic from I/O Operations**

Move all business logic (calculations, operations, storage) into classes or functions that don’t directly interact with the user or the file system.

#### **1.1 Create a Service Layer**

Create a `CalculatorService` class that handles all the operations. This layer will interact with your `operations.py` and `storage.py` modules but not with `io_handler.py`.

```python
# calculator_service.py

import operations
import storage

class CalculatorService:
    def __init__(self):
        self.operations_map = {
            '1': operations.add,
            '2': operations.subtract,
            '3': operations.multiply,
            '4': operations.divide,
            '5': operations.present_value
        }

    def perform_operation(self, operation_code, *args):
        if operation_code in self.operations_map:
            return self.operations_map[operation_code](*args)
        else:
            return "Invalid operation"

    def save_calculation(self, calculation):
        storage.save_calculation(calculation)

    def load_calculations(self):
        return storage.load_calculations()
```

#### **1.2 Modify `calculator.py` to Use the Service Layer**

Refactor the `calculator.py` to use the `CalculatorService` for its operations. This will make it easier to reuse the service layer when building a web app.

```python
# calculator.py

import io_handler
from calculator_service import CalculatorService

def main():
    service = CalculatorService()

    while True:
        choice = io_handler.get_operation_choice()

        if choice in service.operations_map:
            if choice == '5':
                fv = float(input("Enter future value: "))
                rate = float(input("Enter rate (as a decimal): "))
                periods = int(input("Enter number of periods: "))
                result = service.perform_operation(choice, fv, rate, periods)
            else:
                num1, num2 = io_handler.get_user_input()
                result = service.perform_operation(choice, num1, num2)

            io_handler.display_result(result)

        elif choice == '6':  # Save Calculation
            num1, num2 = io_handler.get_user_input()
            operation_choice = io_handler.get_operation_choice()
            result = service.perform_operation(operation_choice, num1, num2)
            service.save_calculation({
                "operation": operation_choice,
                "num1": num1,
                "num2": num2,
                "result": result
            })
            io_handler.display_result("Calculation saved.")

        elif choice == '7':  # Load Calculations
            calculations = service.load_calculations()
            io_handler.display_calculations(calculations)

        else:
            print("Invalid input")

if __name__ == "__main__":
    main()
```

### **2. Prepare for Web Integration:**

#### **Step 2: Separate the Data Access Layer (DAL)**
   - **Why:** In a web app, you might switch from using a JSON file to a database (e.g., SQLite, PostgreSQL).
   - **How:** Create an abstract interface for storage that can be implemented with different backends.

```python
# storage.py

import json
import os

class JSONStorage:
    FILE_PATH = 'calculations.json'

    @staticmethod
    def save_calculation(calculation):
        data = JSONStorage.load_calculations()
        data.append(calculation)
        with open(JSONStorage.FILE_PATH, 'w') as f:
            json.dump(data, f)

    @staticmethod
    def load_calculations():
        if not os.path.exists(JSONStorage.FILE_PATH):
            return []
        with open(JSONStorage.FILE_PATH, 'r') as f:
            return json.load(f)
```

#### **Step 3: Use Dependency Injection for Storage**

Modify the `CalculatorService` to accept any storage implementation, making it easy to swap out JSON for a database when needed.

```python
# calculator_service.py

class CalculatorService:
    def __init__(self, storage_backend):
        self.operations_map = {
            '1': operations.add,
            '2': operations.subtract,
            '3': operations.multiply,
            '4': operations.divide,
            '5': operations.present_value
        }
        self.storage = storage_backend

    def perform_operation(self, operation_code, *args):
        if operation_code in self.operations_map:
            return self.operations_map[operation_code](*args)
        else:
            return "Invalid operation"

    def save_calculation(self, calculation):
        self.storage.save_calculation(calculation)

    def load_calculations(self):
        return self.storage.load_calculations()
```

#### **Step 4: Create a Web-Friendly Interface**

Create a new module for handling HTTP requests and responses, such as `web_interface.py`. If you use a web framework like Flask or Django, this module will interact with `CalculatorService` to handle web requests.

```python
# web_interface.py (using Flask as an example)

from flask import Flask, request, jsonify
from calculator_service import CalculatorService
from storage import JSONStorage

app = Flask(__name__)
service = CalculatorService(JSONStorage())

@app.route('/calculate', methods=['POST'])
def calculate():
    data = request.json
    operation_code = data.get('operation')
    args = data.get('args', [])
    result = service.perform_operation(operation_code, *args)
    return jsonify({"result": result})

@app.route('/save', methods=['POST'])
def save_calculation():
    data = request.json
    service.save_calculation(data)
    return jsonify({"status": "saved"})

@app.route('/load', methods=['GET'])
def load_calculations():
    calculations = service.load_calculations()
    return jsonify(calculations)

if __name__ == "__main__":
    app.run(debug=True)
```

### **3. Additional Considerations for Future Web Development:**

1. **Session Management:** Consider how user sessions will be managed in the web app (e.g., using cookies or tokens).
2. **Error Handling:** Implement robust error handling for web requests.
3. **Security:** Secure the web app by validating and sanitizing user input, especially when dealing with databases.
4. **Scalability:** Plan for scaling the storage solution (e.g., switching from JSON to a scalable database).

### **Conclusion:**

This refactor prepares your application for conversion into a web application by separating concerns, making your logic reusable, and ensuring that your data storage can be easily swapped out for different implementations. When you're ready to move to a web framework, the transition will be much smoother.



# Sixth Prompt (Coding Principles):
Could you go into detail about coding principles, such as SOLID principles, DRY, KISS, YAGNI, Law of Demeter, Composition over inheritance, Encapsulation, High Cohesion, Low Coupling, modular design, and OOP principles?

# AI Feedback from Sixth Prompt (Coding Principles):
Certainly! Here’s a detailed explanation of each coding principle, including how they contribute to writing clean, maintainable, and scalable code.

### 1. **SOLID Principles**
The SOLID principles are a set of five design principles in object-oriented programming that help developers write code that is easy to maintain and extend.

#### **S - Single Responsibility Principle (SRP)**
- **Definition:** A class should have only one reason to change, meaning it should have only one job or responsibility.
- **Example:** In a calculator program, a class that handles mathematical operations should not also handle user input or data storage. Instead, you should separate these concerns into different classes or modules.
- **Benefit:** Reduces the complexity of individual classes and makes it easier to locate and fix issues.

#### **O - Open/Closed Principle (OCP)**
- **Definition:** Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.
- **Example:** If you want to add new operations to a calculator, you should be able to do so by adding new classes or functions rather than modifying existing ones.
- **Benefit:** Prevents breaking existing code when adding new features, making the codebase more stable.

#### **L - Liskov Substitution Principle (LSP)**
- **Definition:** Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.
- **Example:** If you have a base class `Operation`, any subclass like `Addition` or `Subtraction` should work in place of `Operation` without causing errors.
- **Benefit:** Ensures that derived classes extend the functionality of the base class without altering its behavior.

#### **I - Interface Segregation Principle (ISP)**
- **Definition:** Clients should not be forced to depend on interfaces they do not use. This means splitting large interfaces into smaller, more specific ones.
- **Example:** Instead of having a single interface for all calculator operations, you could have separate interfaces for `BasicOperations` and `FinancialOperations`.
- **Benefit:** Reduces the impact of changes in interfaces, as clients only need to know about the methods they actually use.

#### **D - Dependency Inversion Principle (DIP)**
- **Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces).
- **Example:** Instead of having your calculator directly depend on a specific storage implementation (like JSON), it should depend on an interface that different storage mechanisms can implement.
- **Benefit:** Makes it easier to swap out low-level components without affecting the rest of the application.

### 2. **DRY (Don't Repeat Yourself)**
- **Definition:** Avoid duplication of code by abstracting out common functionality that is used in multiple places.
- **Example:** If you have a calculation logic used in multiple operations, extract it into a helper function or a service class.
- **Benefit:** Simplifies code maintenance, as changes need to be made in only one place.

### 3. **KISS (Keep It Simple, Stupid)**
- **Definition:** Aim to keep your code as simple as possible. Complexity should only be introduced when absolutely necessary.
- **Example:** When implementing calculator functions, start with simple arithmetic operations before moving on to more complex financial calculations.
- **Benefit:** Easier to read, understand, and maintain, reducing the likelihood of bugs.

### 4. **YAGNI (You Aren't Gonna Need It)**
- **Definition:** Don’t add functionality until it is necessary. Avoid overengineering by including features that may never be used.
- **Example:** Don’t implement advanced financial operations like mortgage calculations unless they are actually required.
- **Benefit:** Keeps the codebase lean and reduces the time spent on maintaining unused code.

### 5. **Law of Demeter (LoD)**
- **Definition:** An object should only talk to its immediate friends, meaning it should only invoke methods of objects it directly interacts with.
- **Example:** In a calculator, the UI component should not directly manipulate data stored in a file but instead use a service layer to interact with the storage.
- **Benefit:** Reduces dependencies between components, making the code more modular and easier to change.

### 6. **Composition Over Inheritance**
- **Definition:** Prefer using composition (combining objects) over inheritance (extending classes) to achieve code reuse.
- **Example:** Instead of creating a large hierarchy of classes for different calculator operations, you can create small, reusable components that can be composed together.
- **Benefit:** Provides greater flexibility, as components can be combined in different ways without being tightly coupled.

### 7. **Encapsulation**
- **Definition:** Hide the internal state and behavior of an object, exposing only what is necessary through a public interface.
- **Example:** In a calculator, the logic for an operation should be encapsulated within the class, with only a method like `execute()` exposed to the rest of the program.
- **Benefit:** Protects the internal state from unintended interference and makes the code easier to understand.

### 8. **High Cohesion**
- **Definition:** Classes and modules should have a single, well-defined purpose. High cohesion means that the responsibilities of a module are closely related.
- **Example:** A class in a calculator program should either handle user input, perform calculations, or manage storage, but not mix these responsibilities.
- **Benefit:** Makes the code more modular and easier to maintain, as each component has a clear responsibility.

### 9. **Low Coupling**
- **Definition:** Components should be as independent as possible, with minimal reliance on one another.
- **Example:** Your calculator operations should be implemented in a way that changing the input method (e.g., from command-line to web-based) does not require changes to the operation logic.
- **Benefit:** Facilitates easier changes and reduces the risk of cascading failures when modifications are made.

### 10. **Modular Design**
- **Definition:** Divide the program into distinct modules that can be developed, tested, and maintained independently.
- **Example:** In the calculator program, different operations, storage mechanisms, and I/O handlers should be placed in separate modules.
- **Benefit:** Enhances code organization and allows different parts of the program to be worked on in parallel, promoting collaboration and scalability.

### 11. **Object-Oriented Programming (OOP) Principles**
OOP principles are the foundation for writing modular and reusable code in an object-oriented language like Python.

#### **Abstraction**
- **Definition:** Simplifying complex reality by modeling classes appropriate to the problem.
- **Example:** A `Calculator` class abstracts away the details of how operations are performed and simply provides a method to execute them.
- **Benefit:** Reduces complexity by focusing on the high-level operations rather than the implementation details.

#### **Inheritance**
- **Definition:** A mechanism to create a new class that is a modified version of an existing class.
- **Example:** You might have a base class `Operation` with subclasses like `Addition`, `Subtraction`, etc.
- **Benefit:** Promotes code reuse and establishes a natural hierarchy among classes.

#### **Polymorphism**
- **Definition:** The ability of different classes to be treated as instances of the same class through a common interface.
- **Example:** All operation classes (`Addition`, `Subtraction`, etc.) could implement a common `execute` method, allowing them to be used interchangeably.
- **Benefit:** Makes the code more flexible and extensible.

### **Conclusion**
These principles collectively ensure that your code is well-structured, easy to maintain, scalable, and flexible enough to accommodate future changes. By adhering to these guidelines, you can create a robust codebase that is easier to test, debug, and extend, ultimately leading to higher quality software development.
