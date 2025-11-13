# C++ Programming Basics - Summary

## Topics Covered
- Data Types
- Functions
- Classes and Objects
- Constructors
- The Main Function

---

## 1. Data Types

Data types tell the computer what kind of data we're working with:

- `int` - whole numbers (e.g., 5, -10, 100)
- `double` - decimal numbers (e.g., 3.14, -2.5, 0.75)
- `float` - decimal numbers (less precise than double)
- `char` - single character (e.g., 'A', 'x')
- `bool` - true or false values
- `string` - text (e.g., "Hello")

---

## 2. Functions

Functions are reusable blocks of code that perform specific tasks.

**Basic function structure:**
```cpp
return_type function_name(parameters) {
    // code here
    return value;
}
```

**Example - Simple calculator functions:**
```cpp
double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    return a / b;
}
```

---

## 3. Classes and Objects

A **class** is a blueprint for creating objects. An **object** is an instance of a class.

**Class structure:**
```cpp
class ClassName {
private:
    // private variables (only accessible inside the class)
    
public:
    // constructor
    // public functions (accessible from outside)
};
```

---

## 4. Calculator Class Example

Here's how we organize our calculator using a class:

```cpp
#include <iostream>
using namespace std;

class Calculator {
private:
    // Private variables (if needed)
    
public:
    // Constructor
    Calculator() {
        cout << "Calculator initialized!" << endl;
    }
    
    // Addition function
    double add(double a, double b) {
        return a + b;
    }
    
    // Subtraction function
    double subtract(double a, double b) {
        return a - b;
    }
    
    // Multiplication function
    double multiply(double a, double b) {
        return a * b;
    }
    
    // Division function
    double divide(double a, double b) {
        if (b != 0) {
            return a / b;
        } else {
            cout << "Error: Cannot divide by zero!" << endl;
            return 0;
        }
    }
};
```

---

## 5. The Main Function

Every C++ program **must** have a `main()` function. This is where the program starts executing.

```cpp
int main() {
    // Create a Calculator object
    Calculator calc;
    
    // Use the calculator
    double num1 = 10.0;
    double num2 = 5.0;
    
    cout << "Addition: " << calc.add(num1, num2) << endl;
    cout << "Subtraction: " << calc.subtract(num1, num2) << endl;
    cout << "Multiplication: " << calc.multiply(num1, num2) << endl;
    cout << "Division: " << calc.divide(num1, num2) << endl;
    
    return 0;
}
```

---

## 6. Complete Example

```cpp
#include <iostream>
using namespace std;

class Calculator {
public:
    // Constructor
    Calculator() {
        cout << "Calculator ready!" << endl;
    }
    
    double add(double a, double b) {
        return a + b;
    }
    
    double subtract(double a, double b) {
        return a - b;
    }
    
    double multiply(double a, double b) {
        return a * b;
    }
    
    double divide(double a, double b) {
        if (b != 0) {
            return a / b;
        } else {
            cout << "Error: Division by zero!" << endl;
            return 0;
        }
    }
};

int main() {
    // Create calculator object
    Calculator calc;
    
    // Test all operations
    cout << "\nTesting with 15 and 3:" << endl;
    cout << "15 + 3 = " << calc.add(15, 3) << endl;
    cout << "15 - 3 = " << calc.subtract(15, 3) << endl;
    cout << "15 * 3 = " << calc.multiply(15, 3) << endl;
    cout << "15 / 3 = " << calc.divide(15, 3) << endl;
    
    return 0;
}
```

**Output:**
```
Calculator ready!

Testing with 15 and 3:
15 + 3 = 18
15 - 3 = 12
15 * 3 = 45
15 / 3 = 5
```

---

## Key Takeaways

1. **Data types** define what kind of information we store
2. **Functions** perform specific tasks and can return values
3. **Classes** organize related functions and data together
4. **Objects** are created from classes using constructors
5. **Main function** is where every C++ program begins
6. Use meaningful names for functions (like `add`, `subtract`)
7. Always check for errors (like division by zero)

---

## 7. Advanced: Supporting Multiple Data Types

We can make our calculator work with different data types (int, double, char, string) using **function overloading** - creating multiple functions with the same name but different parameter types.

```cpp
#include <iostream>
#include <string>
using namespace std;

class AdvancedCalculator {
public:
    AdvancedCalculator() {
        cout << "Advanced Calculator ready!" << endl;
    }
    
    // Functions for integers
    int add(int a, int b) {
        cout << "Using integer addition" << endl;
        return a + b;
    }
    
    int subtract(int a, int b) {
        cout << "Using integer subtraction" << endl;
        return a - b;
    }
    
    int multiply(int a, int b) {
        cout << "Using integer multiplication" << endl;
        return a * b;
    }
    
    int divide(int a, int b) {
        cout << "Using integer division" << endl;
        if (b != 0) {
            return a / b;
        } else {
            cout << "Error: Division by zero!" << endl;
            return 0;
        }
    }
    
    // Functions for doubles
    double add(double a, double b) {
        cout << "Using double addition" << endl;
        return a + b;
    }
    
    double subtract(double a, double b) {
        cout << "Using double subtraction" << endl;
        return a - b;
    }
    
    double multiply(double a, double b) {
        cout << "Using double multiplication" << endl;
        return a * b;
    }
    
    double divide(double a, double b) {
        cout << "Using double division" << endl;
        if (b != 0) {
            return a / b;
        } else {
            cout << "Error: Division by zero!" << endl;
            return 0.0;
        }
    }
    
    // String concatenation (like addition for strings)
    string add(string a, string b) {
        cout << "Using string concatenation" << endl;
        return a + b;
    }
    
    // Character operations (using ASCII values)
    int add(char a, char b) {
        cout << "Using character addition (ASCII values)" << endl;
        return (int)a + (int)b;
    }
    
    char getNextChar(char c) {
        cout << "Getting next character" << endl;
        return c + 1;
    }
};

int main() {
    AdvancedCalculator calc;
    
    cout << "\n--- Integer Operations ---" << endl;
    cout << "10 + 5 = " << calc.add(10, 5) << endl;
    cout << "10 - 5 = " << calc.subtract(10, 5) << endl;
    cout << "10 * 5 = " << calc.multiply(10, 5) << endl;
    cout << "10 / 5 = " << calc.divide(10, 5) << endl;
    
    cout << "\n--- Double Operations ---" << endl;
    cout << "10.5 + 3.2 = " << calc.add(10.5, 3.2) << endl;
    cout << "10.5 - 3.2 = " << calc.subtract(10.5, 3.2) << endl;
    cout << "10.5 * 3.2 = " << calc.multiply(10.5, 3.2) << endl;
    cout << "10.5 / 3.2 = " << calc.divide(10.5, 3.2) << endl;
    
    cout << "\n--- String Operations ---" << endl;
    cout << "\"Hello\" + \" World\" = " << calc.add("Hello", " World") << endl;
    cout << "\"FRC\" + \" 2025\" = " << calc.add("FRC", " 2025") << endl;
    
    cout << "\n--- Character Operations ---" << endl;
    cout << "'A' + 'B' (ASCII sum) = " << calc.add('A', 'B') << endl;
    cout << "Next char after 'A' = " << calc.getNextChar('A') << endl;
    cout << "Next char after 'Z' = " << calc.getNextChar('Z') << endl;
    
    return 0;
}
```

**Output:**
```
Advanced Calculator ready!

--- Integer Operations ---
Using integer addition
10 + 5 = 15
Using integer subtraction
10 - 5 = 5
Using integer multiplication
10 * 5 = 50
Using integer division
10 / 5 = 2

--- Double Operations ---
Using double addition
10.5 + 3.2 = 13.7
Using double subtraction
10.5 - 3.2 = 7.3
Using double multiplication
10.5 * 3.2 = 33.6
Using double division
10.5 / 3.2 = 3.28125

--- String Operations ---
Using string concatenation
"Hello" + " World" = Hello World
Using string concatenation
"FRC" + " 2025" = FRC 2025

--- Character Operations ---
Using character addition (ASCII values)
'A' + 'B' (ASCII sum) = 131
Getting next character
Next char after 'A' = B
Getting next character
Next char after 'Z' = [
```

### What is Function Overloading?

**Function overloading** means having multiple functions with the same name but different parameter types. The compiler automatically chooses the right function based on what data types you pass in:

- `calc.add(10, 5)` → calls the `int` version
- `calc.add(10.5, 3.2)` → calls the `double` version  
- `calc.add("Hello", " World")` → calls the `string` version
- `calc.add('A', 'B')` → calls the `char` version

This is very useful in robotics! For example, you might have a `setSpeed()` function that accepts either a percentage (double) or a raw value (int).

---