# FRC Robot Programming 

## Topics Covered
- Data Types in Robotics
- Functions for Robot Control
- Classes and Objects (Subsystems)
- Constructors for Hardware
- Control Flow (Autonomous & TeleOp)
- Arrays for Sensors
- Pointers for Hardware Objects
- References for Efficient Programming
- The Main Robot Code

---

## 1. Data Types in Robotics

Data types tell the computer what kind of robot data we're working with:

- `bool` - true/false (button pressed, limit switch triggered)
- `int` - whole numbers (encoder ticks, position targets)
- `double` - decimal numbers (motor speeds 0.0-1.0, angles)
- `char` - single character (rarely used in FRC)
- `string` - text (driver station messages, subsystem names)

**Example:**
```cpp
bool intakeRunning = true;
int targetPosition = 1000;  // encoder ticks
double motorSpeed = 0.75;   // 75% speed
string robotName = "Team Robot";
```

---

## 2. Functions for Robot Control

Functions perform specific robot tasks and can be reused throughout your code.

**Basic function structure:**
```cpp
return_type function_name(parameters) {
    // code here
    return value;
}
```

**Robot Function Examples:**
```cpp
// Start the intake motor
void startIntake() {
    cout << "Intake motor started" << endl;
}

// Set drive speed
void setDriveSpeed(double speed) {
    cout << "Drive speed set to: " << speed << endl;
}

// Check if at target position
bool isAtTarget(int currentPosition, int targetPosition) {
    int tolerance = 10;
    return abs(currentPosition - targetPosition) < tolerance;
}

// Calculate distance from encoder ticks
double getDistance(int encoderTicks) {
    double wheelDiameter = 6.0;  // inches
    double ticksPerRotation = 2048;
    double rotations = encoderTicks / ticksPerRotation;
    return rotations * wheelDiameter * 3.14159;
}
```

---

## 3. Classes - Robot Subsystems

A **class** is a blueprint for robot subsystems. Each subsystem is an **object** created from a class.

**Why use classes in FRC?**
- Organize code by subsystem (drivetrain, intake, shooter)
- Keep hardware and control logic together
- Reuse subsystems across different robots

**Class structure:**
```cpp
class SubsystemName {
private:
    // Private variables (hardware, internal state)
    
public:
    // Constructor
    // Public functions (commands)
};
```

---

## 4. Basic Robot Subsystem Example - Intake

```cpp
#include <iostream>
using namespace std;

class Intake {
private:
    bool motorRunning;
    double motorSpeed;
    
public:
    // Constructor - initializes the subsystem
    Intake() {
        motorRunning = false;
        motorSpeed = 0.0;
        cout << "Intake subsystem initialized!" << endl;
    }
    
    // Start intake
    void start() {
        motorRunning = true;
        motorSpeed = 0.8;
        cout << "Intake started at " << motorSpeed << endl;
    }
    
    // Stop intake
    void stop() {
        motorRunning = false;
        motorSpeed = 0.0;
        cout << "Intake stopped" << endl;
    }
    
    // Reverse intake (outtake)
    void reverse() {
        motorRunning = true;
        motorSpeed = -0.5;
        cout << "Intake reversed at " << motorSpeed << endl;
    }
    
    // Check if running
    bool isRunning() {
        return motorRunning;
    }
    
    // Get current speed
    double getSpeed() {
        return motorSpeed;
    }
};
```

---

## 5. The Main Robot Function

Every robot program has a main function where everything starts. In FRC, this connects to the robot framework.

```cpp
int main() {
    cout << "=== Robot Program Starting ===" << endl;
    
    // Create intake subsystem object
    Intake intake;
    
    // Simulate button presses
    cout << "\nDriver presses intake button..." << endl;
    intake.start();
    
    cout << "Is intake running? " << (intake.isRunning() ? "YES" : "NO") << endl;
    
    cout << "\nDriver releases button..." << endl;
    intake.stop();
    
    cout << "\nDriver presses reverse button..." << endl;
    intake.reverse();
    
    return 0;
}
```

**Output:**
```
=== Robot Program Starting ===
Intake subsystem initialized!

Driver presses intake button...
Intake started at 0.8
Is intake running? YES

Driver releases button...
Intake stopped

Driver presses reverse button...
Intake reversed at -0.5
```

---

## 6. Control Flow - Robot Decision Making

### If/Else for Button Control

```cpp
class Drivetrain {
private:
    double leftSpeed;
    double rightSpeed;
    
public:
    Drivetrain() : leftSpeed(0), rightSpeed(0) {
        cout << "Drivetrain initialized" << endl;
    }
    
    void processJoystick(double joystickY, double joystickX) {
        // Deadband - ignore small joystick movements
        if (abs(joystickY) < 0.1) {
            joystickY = 0;
        }
        if (abs(joystickX) < 0.1) {
            joystickX = 0;
        }
        
        // Calculate arcade drive
        leftSpeed = joystickY + joystickX;
        rightSpeed = joystickY - joystickX;
        
        // Limit to -1.0 to 1.0
        if (leftSpeed > 1.0) leftSpeed = 1.0;
        if (leftSpeed < -1.0) leftSpeed = -1.0;
        if (rightSpeed > 1.0) rightSpeed = 1.0;
        if (rightSpeed < -1.0) rightSpeed = -1.0;
        
        cout << "Left: " << leftSpeed << ", Right: " << rightSpeed << endl;
    }
};
```

### Switch Statement for Autonomous Selection

```cpp
class AutonomousController {
public:
    void selectAutonomous(int mode) {
        cout << "\n=== Autonomous Mode Selected ===" << endl;
        
        switch(mode) {
            case 1:
                cout << "Mode 1: Drive Forward and Shoot" << endl;
                driveForwardAndShoot();
                break;
            case 2:
                cout << "Mode 2: Drive Backward" << endl;
                driveBackward();
                break;
            case 3:
                cout << "Mode 3: Turn and Score" << endl;
                turnAndScore();
                break;
            case 4:
                cout << "Mode 4: Do Nothing (Safe)" << endl;
                break;
            default:
                cout << "Invalid mode! Using safe mode." << endl;
                break;
        }
    }
    
private:
    void driveForwardAndShoot() {
        cout << "  - Driving forward 2 meters" << endl;
        cout << "  - Shooting game piece" << endl;
    }
    
    void driveBackward() {
        cout << "  - Driving backward 1 meter" << endl;
    }
    
    void turnAndScore() {
        cout << "  - Turning 90 degrees" << endl;
        cout << "  - Scoring game piece" << endl;
    }
};
```

### While Loop - Move to Target Position

```cpp
class Elevator {
private:
    int currentPosition;
    
public:
    Elevator() : currentPosition(0) {
        cout << "Elevator initialized at position 0" << endl;
    }
    
    void moveToPosition(int targetPosition) {
        cout << "\nMoving elevator to position: " << targetPosition << endl;
        
        // Simulate movement with a loop
        while (currentPosition != targetPosition) {
            if (currentPosition < targetPosition) {
                currentPosition += 50;  // Move up
                if (currentPosition > targetPosition) {
                    currentPosition = targetPosition;
                }
            } else {
                currentPosition -= 50;  // Move down
                if (currentPosition < targetPosition) {
                    currentPosition = targetPosition;
                }
            }
            
            cout << "  Current position: " << currentPosition << endl;
        }
        
        cout << "Target reached!" << endl;
    }
    
    int getPosition() {
        return currentPosition;
    }
};
```

### For Loop - Check Multiple Sensors

```cpp
class SensorArray {
public:
    void checkAllSensors() {
        cout << "\n=== Checking All Sensors ===" << endl;
        
        // Check 4 distance sensors
        for (int i = 0; i < 4; i++) {
            int distance = readSensor(i);
            cout << "Sensor " << i << ": " << distance << " cm";
            
            if (distance < 20) {
                cout << " - OBSTACLE DETECTED!" << endl;
            } else {
                cout << " - Clear" << endl;
            }
        }
    }
    
private:
    int readSensor(int sensorNum) {
        // Simulate sensor reading
        int readings[4] = {45, 15, 60, 25};
        return readings[sensorNum];
    }
};
```

---

## 7. Arrays - Managing Multiple Sensors and Motors

### Storing Encoder Values

```cpp
class Drivetrain {
private:
    int encoderValues[4];  // FL, FR, BL, BR
    
public:
    Drivetrain() {
        cout << "Drivetrain initialized" << endl;
        
        // Initialize all encoders to 0
        for (int i = 0; i < 4; i++) {
            encoderValues[i] = 0;
        }
    }
    
    void updateEncoders() {
        // Simulate encoder readings
        for (int i = 0; i < 4; i++) {
            encoderValues[i] += 100;  // Increment by 100 ticks
        }
    }
    
    void displayEncoders() {
        string positions[4] = {"Front Left", "Front Right", "Back Left", "Back Right"};
        
        cout << "\n=== Encoder Readings ===" << endl;
        for (int i = 0; i < 4; i++) {
            cout << positions[i] << ": " << encoderValues[i] << " ticks" << endl;
        }
    }
    
    int getAveragePosition() {
        int sum = 0;
        for (int i = 0; i < 4; i++) {
            sum += encoderValues[i];
        }
        return sum / 4;
    }
    
    void resetEncoders() {
        for (int i = 0; i < 4; i++) {
            encoderValues[i] = 0;
        }
        cout << "All encoders reset" << endl;
    }
};
```

### Button State Array

```cpp
class DriverController {
private:
    bool buttons[12];  // Xbox controller has 12 buttons
    
public:
    DriverController() {
        // Initialize all buttons as not pressed
        for (int i = 0; i < 12; i++) {
            buttons[i] = false;
        }
    }
    
    void updateButton(int buttonNum, bool pressed) {
        if (buttonNum >= 0 && buttonNum < 12) {
            buttons[buttonNum] = pressed;
        }
    }
    
    bool isButtonPressed(int buttonNum) {
        if (buttonNum >= 0 && buttonNum < 12) {
            return buttons[buttonNum];
        }
        return false;
    }
    
    int countPressedButtons() {
        int count = 0;
        for (int i = 0; i < 12; i++) {
            if (buttons[i]) {
                count++;
            }
        }
        return count;
    }
    
    void displayButtonStates() {
        cout << "\n=== Button States ===" << endl;
        for (int i = 0; i < 12; i++) {
            cout << "Button " << (i + 1) << ": " 
                 << (buttons[i] ? "PRESSED" : "Released") << endl;
        }
    }
};
```

---

## 8. Pointers - Hardware Objects in FRC

### Why Pointers in FRC?

**FRC hardware objects (motors, sensors) are created using pointers!** This is how WPILib works.

**Key concepts:**
- `new` creates an object dynamically
- `->` accesses methods through a pointer
- `delete` cleans up when done (though in FRC, objects usually last the whole match)

### Creating Motor Controllers with Pointers

```cpp
#include <iostream>
#include <string>
using namespace std;

// Simulate a motor controller class (like Spark Max or Talon FX)
class MotorController {
private:
    int canID;
    double speed;
    string type;
    
public:
    MotorController(int id, string motorType) : canID(id), speed(0), type(motorType) {
        cout << type << " motor (ID: " << canID << ") initialized" << endl;
    }
    
    ~MotorController() {
        cout << type << " motor (ID: " << canID << ") destroyed" << endl;
    }
    
    void set(double newSpeed) {
        speed = newSpeed;
        cout << type << " (ID: " << canID << ") speed set to: " << speed << endl;
    }
    
    double get() {
        return speed;
    }
    
    void stop() {
        speed = 0;
        cout << type << " (ID: " << canID << ") stopped" << endl;
    }
};

class Drivetrain {
private:
    MotorController* leftMotor;   // Pointer to left motor
    MotorController* rightMotor;  // Pointer to right motor
    
public:
    Drivetrain() {
        cout << "\n=== Initializing Drivetrain ===" << endl;
        
        // Create motor objects using new (dynamic allocation)
        leftMotor = new MotorController(1, "Left Drive");
        rightMotor = new MotorController(2, "Right Drive");
    }
    
    ~Drivetrain() {
        // Clean up motors
        delete leftMotor;
        delete rightMotor;
    }
    
    // Use -> operator to access motor methods
    void drive(double leftSpeed, double rightSpeed) {
        leftMotor->set(leftSpeed);
        rightMotor->set(rightSpeed);
    }
    
    void stop() {
        leftMotor->stop();
        rightMotor->stop();
    }
    
    void displaySpeeds() {
        cout << "\nCurrent speeds:" << endl;
        cout << "  Left: " << leftMotor->get() << endl;
        cout << "  Right: " << rightMotor->get() << endl;
    }
};
```

### Multiple Subsystem Pointers

```cpp
class Robot {
private:
    Drivetrain* drivetrain;
    Intake* intake;
    Elevator* elevator;
    
public:
    Robot() {
        cout << "\n=== Robot Initialization ===" << endl;
        
        // Create all subsystems
        drivetrain = new Drivetrain();
        intake = new Intake();
        elevator = new Elevator();
        
        cout << "Robot ready!" << endl;
    }
    
    ~Robot() {
        cout << "\n=== Robot Shutdown ===" << endl;
        delete drivetrain;
        delete intake;
        delete elevator;
    }
    
    void autonomousInit() {
        cout << "\n=== Autonomous Started ===" << endl;
        drivetrain->drive(0.5, 0.5);
        intake->start();
    }
    
    void teleopInit() {
        cout << "\n=== TeleOp Started ===" << endl;
        drivetrain->stop();
        intake->stop();
    }
};
```

---

## 9. References - Efficient Robot Programming

### Pass by Reference for Subsystems

```cpp
class RobotController {
public:
    // Pass by value (BAD - makes a copy of entire drivetrain!)
    void controlDriveBad(Drivetrain dt, double speed) {
        // This won't affect the original drivetrain
        // dt.drive(speed, speed);
    }
    
    // Pass by reference (GOOD - uses original drivetrain)
    void controlDriveGood(Drivetrain& dt, double speed) {
        dt.drive(speed, speed);
    }
    
    // Pass by pointer (ALSO GOOD - common in FRC)
    void controlDrivePointer(Drivetrain* dt, double speed) {
        dt->drive(speed, speed);
    }
};
```

### Reference Example - Update Sensor Values

```cpp
class SensorManager {
public:
    // Update multiple values at once using references
    void readAllSensors(int& distance, double& angle, bool& limitSwitch) {
        distance = 45;        // cm
        angle = 90.5;         // degrees
        limitSwitch = true;   // pressed
    }
    
    // Swap two motor speeds
    void swapMotorSpeeds(double& motor1, double& motor2) {
        double temp = motor1;
        motor1 = motor2;
        motor2 = temp;
    }
};

int main() {
    SensorManager sensors;
    
    int dist;
    double ang;
    bool limit;
    
    // Read all sensors at once
    sensors.readAllSensors(dist, ang, limit);
    
    cout << "Distance: " << dist << " cm" << endl;
    cout << "Angle: " << ang << " degrees" << endl;
    cout << "Limit Switch: " << (limit ? "PRESSED" : "Open") << endl;
    
    // Swap speeds
    double leftSpeed = 0.5, rightSpeed = 0.8;
    cout << "\nBefore swap: Left=" << leftSpeed << ", Right=" << rightSpeed << endl;
    
    sensors.swapMotorSpeeds(leftSpeed, rightSpeed);
    cout << "After swap: Left=" << leftSpeed << ", Right=" << rightSpeed << endl;
    
    return 0;
}
```

---

## 10. Complete FRC Robot Example

```cpp
#include <iostream>
#include <string>
using namespace std;

// Motor Controller Class
class MotorController {
private:
    int canID;
    double speed;
    string name;
    
public:
    MotorController(int id, string motorName) : canID(id), speed(0), name(motorName) {
        cout << "Motor '" << name << "' (ID: " << canID << ") created" << endl;
    }
    
    void set(double newSpeed) {
        speed = newSpeed;
    }
    
    double get() {
        return speed;
    }
    
    void stop() {
        speed = 0;
    }
    
    string getName() {
        return name;
    }
};

// Drivetrain Subsystem
class Drivetrain {
private:
    MotorController* leftMotor;
    MotorController* rightMotor;
    int encoderValues[2];  // Left and right encoders
    
public:
    Drivetrain() {
        cout << "\n=== Drivetrain Initialization ===" << endl;
        leftMotor = new MotorController(1, "Left Drive");
        rightMotor = new MotorController(2, "Right Drive");
        
        encoderValues[0] = 0;
        encoderValues[1] = 0;
    }
    
    ~Drivetrain() {
        delete leftMotor;
        delete rightMotor;
    }
    
    void arcadeDrive(double forward, double turn) {
        // Apply deadband
        if (abs(forward) < 0.1) forward = 0;
        if (abs(turn) < 0.1) turn = 0;
        
        double leftSpeed = forward + turn;
        double rightSpeed = forward - turn;
        
        // Limit speeds
        if (leftSpeed > 1.0) leftSpeed = 1.0;
        if (leftSpeed < -1.0) leftSpeed = -1.0;
        if (rightSpeed > 1.0) rightSpeed = 1.0;
        if (rightSpeed < -1.0) rightSpeed = -1.0;
        
        leftMotor->set(leftSpeed);
        rightMotor->set(rightSpeed);
        
        // Update encoders (simulate)
        encoderValues[0] += (int)(leftSpeed * 100);
        encoderValues[1] += (int)(rightSpeed * 100);
    }
    
    void stop() {
        leftMotor->stop();
        rightMotor->stop();
    }
    
    void displayStatus() {
        cout << "\n--- Drivetrain Status ---" << endl;
        cout << "Left Speed: " << leftMotor->get() << endl;
        cout << "Right Speed: " << rightMotor->get() << endl;
        cout << "Left Encoder: " << encoderValues[0] << endl;
        cout << "Right Encoder: " << encoderValues[1] << endl;
    }
    
    void resetEncoders() {
        encoderValues[0] = 0;
        encoderValues[1] = 0;
        cout << "Encoders reset" << endl;
    }
};

// Intake Subsystem
class Intake {
private:
    MotorController* intakeMotor;
    bool hasGamePiece;
    
public:
    Intake() {
        cout << "\n=== Intake Initialization ===" << endl;
        intakeMotor = new MotorController(5, "Intake");
        hasGamePiece = false;
    }
    
    ~Intake() {
        delete intakeMotor;
    }
    
    void run() {
        intakeMotor->set(0.8);
        cout << "Intake running" << endl;
    }
    
    void reverse() {
        intakeMotor->set(-0.6);
        cout << "Intake reversing (outtake)" << endl;
    }
    
    void stop() {
        intakeMotor->stop();
        cout << "Intake stopped" << endl;
    }
    
    void setGamePieceDetected(bool detected) {
        hasGamePiece = detected;
        if (detected) {
            cout << "Game piece acquired!" << endl;
        }
    }
    
    bool hasGamePieceDetected() {
        return hasGamePiece;
    }
};

// Autonomous Controller
class AutonomousController {
private:
    Drivetrain* drivetrain;
    Intake* intake;
    
public:
    AutonomousController(Drivetrain* dt, Intake* in) : drivetrain(dt), intake(in) {}
    
    void runMode(int mode) {
        cout << "\n=== Running Autonomous Mode " << mode << " ===" << endl;
        
        switch(mode) {
            case 1:
                mode1_DriveAndShoot();
                break;
            case 2:
                mode2_SimpleBackup();
                break;
            case 3:
                mode3_Mobility();
                break;
            default:
                cout << "Invalid autonomous mode!" << endl;
                break;
        }
    }
    
private:
    void mode1_DriveAndShoot() {
        cout << "Step 1: Starting intake" << endl;
        intake->run();
        
        cout << "Step 2: Driving forward" << endl;
        drivetrain->arcadeDrive(0.5, 0);
        
        cout << "Step 3: Stopping" << endl;
        drivetrain->stop();
        intake->stop();
    }
    
    void mode2_SimpleBackup() {
        cout << "Step 1: Backing up" << endl;
        drivetrain->arcadeDrive(-0.3, 0);
        
        cout << "Step 2: Stopping" << endl;
        drivetrain->stop();
    }
    
    void mode3_Mobility() {
        cout << "Step 1: Leave starting zone" << endl;
        drivetrain->arcadeDrive(0.6, 0);
        
        cout << "Step 2: Stop outside zone" << endl;
        drivetrain->stop();
    }
};

// Main Robot Class
class Robot {
private:
    Drivetrain* drivetrain;
    Intake* intake;
    AutonomousController* autoController;
    
    bool autonomousMode;
    int selectedAuto;
    
public:
    Robot() {
        cout << "\n╔════════════════════════════════════╗" << endl;
        cout << "║     ROBOT INITIALIZATION START     ║" << endl;
        cout << "╚════════════════════════════════════╝" << endl;
        
        // Create subsystems
        drivetrain = new Drivetrain();
        intake = new Intake();
        autoController = new AutonomousController(drivetrain, intake);
        
        autonomousMode = false;
        selectedAuto = 1;
        
        cout << "\n✓ Robot initialization complete!" << endl;
    }
    
    ~Robot() {
        cout << "\n=== Robot Shutdown ===" << endl;
        delete autoController;
        delete intake;
        delete drivetrain;
    }
    
    void selectAutonomous(int mode) {
        selectedAuto = mode;
        cout << "\nAutonomous mode " << mode << " selected" << endl;
    }
    
    void autonomousInit() {
        cout << "\n\n╔════════════════════════════════════╗" << endl;
        cout << "║       AUTONOMOUS MODE STARTED      ║" << endl;
        cout << "╚════════════════════════════════════╝" << endl;
        
        autonomousMode = true;
        drivetrain->resetEncoders();
        autoController->runMode(selectedAuto);
    }
    
    void teleopInit() {
        cout << "\n\n╔════════════════════════════════════╗" << endl;
        cout << "║         TELEOP MODE STARTED        ║" << endl;
        cout << "╚════════════════════════════════════╝" << endl;
        
        autonomousMode = false;
        drivetrain->stop();
        intake->stop();
    }
    
    void teleopPeriodic(double joystickY, double joystickX, bool intakeButton, bool outtakeButton) {
        // Drive control
        drivetrain->arcadeDrive(joystickY, joystickX);
        
        // Intake control
        if (intakeButton) {
            intake->run();
        } else if (outtakeButton) {
            intake->reverse();
        } else {
            intake->stop();
        }
    }
    
    void displayStatus() {
        cout << "\n╔════════════════════════════════════╗" << endl;
        cout << "║          ROBOT STATUS              ║" << endl;
        cout << "╚════════════════════════════════════╝" << endl;
        
        cout << "Mode: " << (autonomousMode ? "AUTONOMOUS" : "TELEOP") << endl;
        
        drivetrain->displayStatus();
        
        cout << "\n--- Intake Status ---" << endl;
        cout << "Has Game Piece: " << (intake->hasGamePieceDetected() ? "YES" : "NO") << endl;
    }
};

// Main Program
int main() {
    // Create robot
    Robot* robot = new Robot();
    
    // Select autonomous mode
    robot->selectAutonomous(1);
    
    // Run autonomous
    robot->autonomousInit();
    robot->displayStatus();
    
    // Switch to teleop
    robot->teleopInit();
    
    // Simulate driver control
    cout << "\n=== Driver Control Simulation ===" << endl;
    
    cout << "\n1. Driver pushes joystick forward" << endl;
    robot->teleopPeriodic(0.7, 0.0, false, false);
    
    cout << "\n2. Driver presses intake button" << endl;
    robot->teleopPeriodic(0.5, 0.0, true, false);
    
    cout << "\n3. Driver turns right" << endl;
    robot->teleopPeriodic(0.5, 0.4, false, false);
    
    cout << "\n4. Driver stops" << endl;
    robot->teleopPeriodic(0.0, 0.0, false, false);
    
    // Display final status
    robot->displayStatus();
    
    // Cleanup
    delete robot;
    
    cout << "\n=== Program Complete ===" << endl;
    
    return 0;
}
```

**Output:**
```
╔════════════════════════════════════╗
║     ROBOT INITIALIZATION START     ║
╚════════════════════════════════════╝

=== Drivetrain Initialization ===
Motor 'Left Drive' (ID: 1) created
Motor 'Right Drive' (ID: 2) created

=== Intake Initialization ===
Motor 'Intake' (ID: 5) created

✓ Robot initialization complete!

Autonomous mode 1 selected


╔════════════════════════════════════╗
║       AUTONOMOUS MODE STARTED      ║
╚════════════════════════════════════╝

Encoders reset

=== Running Autonomous Mode 1 ===
Step 1: Starting intake
Intake running
Step 2: Driving forward
Step 3: Stopping
Intake stopped

╔════════════════════════════════════╗
║          ROBOT STATUS              ║
╚════════════════════════════════════╝
Mode: AUTONOMOUS

--- Drivetrain Status ---
Left Speed: 0
Right Speed: 0
Left Encoder: 50
Right Encoder: 50

--- Intake Status ---
Has Game Piece: NO
```

---

## 11. Key Concepts Summary

### Data Types in FRC
| Type | Use Case | Example |
|------|----------|---------|
| `bool` | Button states, limit switches | `bool limitPressed = true;` |
| `int` | Encoder values, CAN IDs | `int encoderTicks = 2048;` |
| `double` | Motor speeds, angles | `double speed = 0.75;` |
| `string` | Subsystem names, messages | `string name = "Drivetrain";` |

### Pointers vs References
| Feature | Pointer | Reference |
|---------|---------|-----------|
| Use in FRC | Hardware objects (`new MotorController()`) | Function parameters |
| Syntax | `Motor* m = new Motor()` | `void control(Motor& m)` |
| Access | `motor->set(0.5)` | `motor.set(0.5)` |
| Can be null | Yes | No |

### Control Flow in FRC
- **if/else**: Button checks, sensor validation, safety limits
- **switch**: Autonomous mode selection, command selection
- **for loop**: Iterate through sensor arrays, check multiple motors
- **while loop**: Wait for target position, autonomous sequences

---