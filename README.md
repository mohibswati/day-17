import logging
import os

logging.basicConfig(
    filename='error_log.txt',
    level=logging.ERROR,
    format='%(asctime)s - %(levelname)s - %(message)s',
    filemode='a'
)

class InvalidInputError(Exception):
    pass

class SimpleCalculator:
    def add(self, a, b):
        try:
            return a + b
        except Exception as e:
            logging.error(f"Add error: {e} | a={a}, b={b}")
            return None

    def subtract(self, a, b):
        try:
            return a - b
        except Exception as e:
            logging.error(f"Subtract error: {e} | a={a}, b={b}")
            return None

    def multiply(self, a, b):
        try:
            return a * b
        except Exception as e:
            logging.error(f"Multiply error: {e} | a={a}, b={b}")
            return None

    def divide(self, a, b):
        try:
            if b == 0:
                raise ValueError("Cannot divide by zero")
            return a / b
        except Exception as e:
            logging.error(f"Divide error: {e} | a={a}, b={b}")
            return None

def read_file_example(file_name):
    try:
        if not os.path.exists(file_name):
            print(f"\nFile '{file_name}' does not exist. Skipping file read.")
            logging.error(f"FileNotFoundError: '{file_name}' not found.")
            return None

        with open(file_name, 'r') as file:
            content = file.read()
            print(f"\nContents of '{file_name}':\n{content}")
            return content

    except Exception as e:
        print(f"\nUnexpected error while reading file: {e}")
        logging.error(f"Unexpected error reading file '{file_name}': {e}")
        return None

def run_calculator_cli():
    calc = SimpleCalculator()
    print("\n--- Simple Python Calculator ---")
    print("Type 'exit' anytime to quit.\n")

    while True:
        try:
            a = input("Enter first number: ")
            if a.lower() == "exit":
                break
            a = float(a)

            op = input("Enter operation (+, -, *, /): ")
            if op.lower() == "exit":
                break

            b = input("Enter second number: ")
            if b.lower() == "exit":
                break
            b = float(b)

            if op == '+':
                result = calc.add(a, b)
            elif op == '-':
                result = calc.subtract(a, b)
            elif op == '*':
                result = calc.multiply(a, b)
            elif op == '/':
                result = calc.divide(a, b)
            else:
                raise InvalidInputError("Invalid operation.")

            if result is not None:
                print(f"Result: {result}\n")
            else:
                print("Calculation failed.\n")

        except ValueError:
            print("Please enter valid numbers.\n")
        except InvalidInputError as e:
            print(f"{e}\n")
        except Exception as e:
            print(f"Unexpected error: {e}\n")
            logging.error(f"CLI loop error: {e}")

if __name__ == "__main__":
    read_file_example('client_requirements.txt')
    run_calculator_cli()

