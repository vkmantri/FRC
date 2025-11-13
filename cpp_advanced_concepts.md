# C++ Programming - Part 2: Control Flow, Arrays, Pointers & References

## Topics Covered
- Control Flow (if/else, switch, loops)
- Arrays and Collections
- Pointers Basics
- References
- Applications in Robotics

---

## 1. Control Flow - if/else Statements

Control flow lets your program make decisions based on conditions.

### Basic if/else
```cpp
#include <iostream>
using namespace std;

int main() {
    int sensorValue = 75;
    
    if (sensorValue > 100) {
        cout << "Sensor value is HIGH" << endl;
    } else if (sensorValue > 50) {
        cout << "Sensor value is MEDIUM" << endl;
    } else {
        cout << "Sensor value is LOW" << endl;
    }
    
    return 0;
}
```

### Robotics Example - Button Control
```cpp
class RobotController {
public:
    void checkButton(bool buttonPressed) {
        if (buttonPressed) {
            cout << "Button pressed - Starting intake motor" << endl;
            // Start motor code here
        } else {
            cout << "Button not pressed - Motor stopped" << endl;
            // Stop motor code here
        }
    }
    
    void checkJoystick(double joystickValue) {
        // joystickValue ranges from -1.0 to 1.0
        if (joystickValue > 0.1) {
            cout << "Moving forward at speed: " << joystickValue << endl;
        } else if (joystickValue < -0.1) {
            cout << "Moving backward at speed: " << joystickValue << endl;
        } else {
            cout << "Stopped (deadband)" << endl;
        }
    }
};
```

---

## 2. Switch Statements

Use `switch` when checking a variable against multiple specific values.

```cpp
class AutonomousMode {
public:
    void selectMode(int mode) {
        switch(mode) {
            case 1:
                cout << "Running: Drive Forward" << endl;
                // Code for mode 1
                break;
            case 2:
                cout << "Running: Turn Left" << endl;
                // Code for mode 2
                break;
            case 3:
                cout << "Running: Shoot and Drive" << endl;
                // Code for mode 3
                break;
            default:
                cout << "Invalid mode selected" << endl;
                break;
        }
    }
};
```

---

## 3. Loops

Loops repeat code multiple times.

### For Loop
```cpp
// Print numbers 0 to 9
for (int i = 0; i < 10; i++) {
    cout << i << " ";
}
cout << endl;

// Robotics example: Read multiple sensors
for (int sensorNum = 0; sensorNum < 5; sensorNum++) {
    cout << "Reading sensor " << sensorNum << endl;
    // Read sensor code here
}
```

### While Loop
```cpp
int encoderValue = 0;
int targetPosition = 1000;

while (encoderValue < targetPosition) {
    cout << "Current position: " << encoderValue << endl;
    encoderValue += 50; // Simulate movement
    // In real robot: motor.set(speed);
}
cout << "Target reached!" << endl;
```

### Do-While Loop
```cpp
int userChoice;
do {
    cout << "Select mode (1-3, 0 to exit): ";
    cin >> userChoice;
    
    if (userChoice > 0 && userChoice <= 3) {
        cout << "Mode " << userChoice << " selected" << endl;
    }
} while (userChoice != 0);
```

---

## 4. Arrays

Arrays store multiple values of the same type in a single variable.

### Basic Array
```cpp
// Declare an array of 5 integers
int sensorReadings[5];

// Initialize with values
int motorSpeeds[4] = {50, 75, 100, 25};

// Access elements (index starts at 0)
cout << "First motor speed: " << motorSpeeds[0] << endl;
cout << "Second motor speed: " << motorSpeeds[1] << endl;

// Modify an element
motorSpeeds[2] = 80;
```

### Looping Through Arrays
```cpp
class SensorArray {
public:
    void readAllSensors() {
        int sensors[6] = {10, 25, 30, 15, 40, 35};
        
        cout << "Reading all sensors:" << endl;
        for (int i = 0; i < 6; i++) {
            cout << "Sensor " << i << ": " << sensors[i] << endl;
        }
    }
    
    int findMaxSensor() {
        int sensors[6] = {10, 25, 30, 15, 40, 35};
        int maxValue = sensors[0];
        
        for (int i = 1; i < 6; i++) {
            if (sensors[i] > maxValue) {
                maxValue = sensors[i];
            }
        }
        
        return maxValue;
    }
};
```

### Multi-Dimensional Arrays
```cpp
// 2D array for game piece positions
int grid[3][3] = {
    {1, 0, 1},
    {0, 1, 0},
    {1, 0, 1}
};

// Access element at row 1, column 2
cout << "Value: " << grid[1][2] << endl;
```

---

## 5. Pointers Basics

A **pointer** is a variable that stores the memory address of another variable.

### Why Pointers Matter in FRC
In FRC programming, hardware objects (motors, sensors) are often accessed through pointers. Understanding pointers helps you work with WPILib effectively.

### Basic Pointer Syntax
```cpp
int main() {
    int speed = 100;
    int* speedPtr;  // Declare a pointer to an integer
    
    speedPtr = &speed;  // & gets the address of speed
    
    cout << "Value of speed: " << speed << endl;
    cout << "Address of speed: " << &speed << endl;
    cout << "Value stored in speedPtr: " << speedPtr << endl;
    cout << "Value pointed to by speedPtr: " << *speedPtr << endl;
    
    return 0;
}
```

**Key Operators:**
- `&` - Address-of operator (gets memory address)
- `*` - Dereference operator (gets value at address)

### Pointer Example
```cpp
void changeValue(int* ptr) {
    *ptr = 200;  // Changes the value at the address
}

int main() {
    int motorSpeed = 100;
    cout << "Before: " << motorSpeed << endl;
    
    changeValue(&motorSpeed);  // Pass the address
    
    cout << "After: " << motorSpeed << endl;
    return 0;
}
```

**Output:**
```
Before: 100
After: 200
```

### Pointers in Robotics (FRC Style)
```cpp
class Motor {
private:
    int speed;
    string name;
    
public:
    Motor(string motorName) : name(motorName), speed(0) {
        cout << "Motor '" << name << "' created" << endl;
    }
    
    void setSpeed(int newSpeed) {
        speed = newSpeed;
        cout << name << " speed set to: " << speed << endl;
    }
    
    int getSpeed() {
        return speed;
    }
};

int main() {
    // Create motor object dynamically (like in FRC)
    Motor* leftMotor = new Motor("Left Drive");
    Motor* rightMotor = new Motor("Right Drive");
    
    // Use pointer with -> operator
    leftMotor->setSpeed(75);
    rightMotor->setSpeed(75);
    
    cout << "Left motor speed: " << leftMotor->getSpeed() << endl;
    
    // Clean up (important!)
    delete leftMotor;
    delete rightMotor;
    
    return 0;
}
```

**Output:**
```
Motor 'Left Drive' created
Motor 'Right Drive' created
Left Drive speed set to: 75
Right Drive speed set to: 75
Left motor speed: 75
```

---

## 6. References

A **reference** is an alias (another name) for an existing variable. Unlike pointers, references cannot be null and must be initialized.

### Basic Reference Syntax
```cpp
int main() {
    int speed = 100;
    int& speedRef = speed;  // speedRef is a reference to speed
    
    cout << "speed: " << speed << endl;
    cout << "speedRef: " << speedRef << endl;
    
    speedRef = 200;  // Changes speed through the reference
    
    cout << "After change:" << endl;
    cout << "speed: " << speed << endl;
    cout << "speedRef: " << speedRef << endl;
    
    return 0;
}
```

**Output:**
```
speed: 100
speedRef: 100
After change:
speed: 200
speedRef: 200
```

### References in Functions
```cpp
// Pass by value (makes a copy)
void changeValueCopy(int value) {
    value = 200;  // Only changes the copy
}

// Pass by reference (modifies original)
void changeValueReference(int& value) {
    value = 200;  // Changes the original variable
}

int main() {
    int motorSpeed = 100;
    
    cout << "Original: " << motorSpeed << endl;
    
    changeValueCopy(motorSpeed);
    cout << "After pass by value: " << motorSpeed << endl;
    
    changeValueReference(motorSpeed);
    cout << "After pass by reference: " << motorSpeed << endl;
    
    return 0;
}
```

**Output:**
```
Original: 100
After pass by value: 100
After pass by reference: 200
```

### Robotics Example with References
```cpp
class DriveController {
private:
    double leftSpeed;
    double rightSpeed;
    
public:
    DriveController() : leftSpeed(0), rightSpeed(0) {}
    
    // Return references to allow modification
    double& getLeftSpeed() {
        return leftSpeed;
    }
    
    double& getRightSpeed() {
        return rightSpeed;
    }
    
    void displaySpeeds() {
        cout << "Left: " << leftSpeed << ", Right: " << rightSpeed << endl;
    }
};

int main() {
    DriveController drive;
    
    // Get references and modify directly
    drive.getLeftSpeed() = 0.75;
    drive.getRightSpeed() = 0.80;
    
    drive.displaySpeeds();
    
    return 0;
}
```

---

## 7. Pointers vs References - Quick Comparison

| Feature | Pointer | Reference |
|---------|---------|-----------|
| Symbol | `*` | `&` |
| Can be null | Yes | No |
| Must be initialized | No | Yes |
| Can be reassigned | Yes | No |
| Access value | `*ptr` | `ref` (direct) |
| Common in FRC | Hardware objects | Function parameters |

**When to use what:**
- **Pointers**: Hardware objects, dynamic memory (motors, sensors in FRC)
- **References**: Function parameters, avoiding copies of large objects

---

## 8. Complete Robotics Example

```cpp
#include <iostream>
using namespace std;

class RobotSubsystem {
private:
    int encoderValues[4];  // Array for 4 encoder readings
    bool limitSwitches[2]; // Array for 2 limit switches
    
public:
    RobotSubsystem() {
        // Initialize arrays
        for (int i = 0; i < 4; i++) {
            encoderValues[i] = 0;
        }
        limitSwitches[0] = false;
        limitSwitches[1] = false;
    }
    
    void updateEncoders(int values[], int size) {
        for (int i = 0; i < size && i < 4; i++) {
            encoderValues[i] = values[i];
        }
    }
    
    void checkLimits() {
        // Control flow example
        if (limitSwitches[0]) {
            cout << "Lower limit reached - stopping motor" << endl;
        } else if (limitSwitches[1]) {
            cout << "Upper limit reached - stopping motor" << endl;
        } else {
            cout << "Within safe range" << endl;
        }
    }
    
    void displayStatus() {
        cout << "\n--- Robot Status ---" << endl;
        
        // Loop through encoders
        for (int i = 0; i < 4; i++) {
            cout << "Encoder " << i << ": " << encoderValues[i] << endl;
        }
        
        // Check limit switches
        cout << "Lower Limit: " << (limitSwitches[0] ? "PRESSED" : "Open") << endl;
        cout << "Upper Limit: " << (limitSwitches[1] ? "PRESSED" : "Open") << endl;
    }
    
    // Reference example - allows external modification
    bool& getLimitSwitch(int index) {
        return limitSwitches[index];
    }
};

int main() {
    RobotSubsystem robot;
    
    // Simulate encoder readings
    int readings[4] = {100, 250, 300, 150};
    robot.updateEncoders(readings, 4);
    
    // Simulate limit switch press using reference
    robot.getLimitSwitch(0) = true;
    
    robot.displayStatus();
    robot.checkLimits();
    
    return 0;
}
```

**Output:**
```
--- Robot Status ---
Encoder 0: 100
Encoder 1: 250
Encoder 2: 300
Encoder 3: 150
Lower Limit: PRESSED
Upper Limit: Open
Lower limit reached - stopping motor
```

---

## Key Takeaways

1. **Control flow** (if/else, switch, loops) lets robots make decisions
2. **Arrays** store multiple sensor readings or motor speeds efficiently
3. **Pointers** store memory addresses - essential for FRC hardware objects
4. **References** provide aliases to variables - great for function parameters
5. Use `->` with pointers to access class members
6. Always `delete` objects created with `new`
7. Pass by reference to avoid copying large objects

---