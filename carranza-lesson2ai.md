NOTE: Using ChatGPT for this lesson

-Inital Prompt:
I want to create a calculator program in python using the procedural paradigm. Initially, the feature will be for addition, subtraction, multiplication, and division. I will add other features such as present value, mortgage payments, etc. later so the design must easily allow this.

-Feedback from inital prompt:
Here's a basic structure for your calculator program that follows the procedural paradigm. This structure will allow you to easily add more features later.

 1. **Define Functions for Basic Operations:**

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

2. **Create a Main Function to Handle User Input:**

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

3. **Adding More Features Later:**

    To add more features later, simply define additional functions (e.g., `def present_value(...):`) and update the `main()` function to include these new operations. The design ensures that expanding functionality is straightforward by adding more options and corresponding function calls.

4. **Example of Adding Present Value Calculation:**

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
