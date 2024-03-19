# CALCULATOR.PY
import math
import re

class Calculator:
    def __init__(self):
        self.operators = {
            '+': self.add,
            '-': self.subtract,
            '*': self.multiply,
            '/': self.divide,
            '^': self.power,
            'sqrt': self.sqrt,
            'sin': math.sin,
            'cos': math.cos,
            'tan': math.tan
        }

    def parse_expression(self, expression):
        for name, func in self.operators.items():
            if name in expression:
                if name in ['sin', 'cos', 'tan', 'sqrt']:
                    match = re.search(f'{name}\((.*?)\)', expression)
                    if match:
                        number = float(match.group(1))
                        result = func(number)
                        return expression.replace(match.group(0), str(result))
                else:
                    parts = expression.split(name)
                    result = func(float(parts[0]), float(parts[1]))
                    return str(result)
        return expression

    def add(self, a, b):
        return a + b

    def subtract(self, a, b):
        return a - b

    def multiply(self, a, b):
        return a * b

    def divide(self, a, b):
        return a / b

    def power(self, a, b):
        return a ** b

    def sqrt(self, x):
        return math.sqrt(x)

    def calculate(self, expression):
        expression = expression.replace(' ', '')
        while any(op in expression for op in self.operators):
            expression = self.parse_expression(expression)
        return expression

def main():
    calculator = Calculator()
    print("Enhanced Calculator. Type 'quit' to exit.")
    while True:
        expression = input("Enter expression: ")
        if expression.lower() == 'quit':
            break
        try:
            result = calculator.calculate(expression)
            print("Result:", result)
        except Exception as e:
            print("Error:", e)

if __name__ == "__main__":
    main()
