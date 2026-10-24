# Webots Commands & Makefile Cheat Sheet (Ubuntu)

This guide provides a quick reference for launching Webots from the Ubuntu terminal, managing builds using Makefiles, and writing standard controller code.

## 1. Official Documentation Links
* **Command Line Arguments:** [Webots CLI Docs](https://cyberbotics.com/doc/guide/starting-webots#command-line-arguments)
* **Makefile Configuration:** [Webots Makefile Docs](https://cyberbotics.com/doc/guide/using-c)
* **C/C++ API Reference:** [Webots Programming Guide](https://cyberbotics.com/doc/guide/controller-programming)

---

## 2. Webots Terminal Commands (Ubuntu CLI)

When working in Ubuntu, you can launch and control Webots directly from the terminal. This is especially useful for automated testing or machine learning scripts.

### Basic Launch Commands
* `webots` : Launches the Webots GUI with the last opened world.
* `webots /path/to/my_world.wbt` : Opens a specific world file.
* `webots --help` : Displays all available command-line arguments.

### Execution Modes
You can force Webots to start in a specific mode (overriding the world file's default):
* `webots --mode=pause my_world.wbt` : Starts the simulation paused.
* `webots --mode=realtime my_world.wbt` : Starts running at normal 1x speed.
* `webots --mode=fast my_world.wbt` : Runs as fast as possible (skips rendering some frames).

### Headless & Automated Modes (Great for CI/CD or Training)
* `webots --no-rendering my_world.wbt` : Runs the simulation without 3D graphics (maximum speed, uses less CPU/GPU).
* `webots --minimize my_world.wbt` : Starts Webots minimized in the Ubuntu taskbar.
* `webots --batch my_world.wbt` : Prevents blocking pop-up windows from interrupting the simulation.

---

## 3. Makefile Commands & Structure

Webots uses GNU Make. To compile your C/C++ controllers, you must navigate to your controller directory in the terminal (`cd my_project/controllers/my_controller`) and use the following commands.

### Make Commands
* `make` : Compiles the controller using standard settings.
* `make clean` : Deletes all compiled binary and object files (useful if you encounter caching issues).
* `make debug` : Compiles the controller with debugging symbols (`-g`), allowing you to use `gdb`.
* `make release` : Compiles with maximum performance optimizations (`-O3`).
* `make profile` : Compiles with profiling instructions (`-pg`) for performance analysis.

### Standard Ubuntu Makefile Template
Create a file exactly named `Makefile` in your controller directory. This minimal template is all you need on Ubuntu, as it links automatically to the Webots libraries.

```makefile
# -----------------------------------------------------------------------------
# Webots Makefile for C/C++ Controllers on Ubuntu
# -----------------------------------------------------------------------------

# Add external include directories (e.g., -I/usr/include/...)
INCLUDE = 

# Add external libraries here (e.g., -lm for math, -lopencv_core)
LIBRARIES = -lm

# Compiler flags
CFLAGS = -Wall
CXXFLAGS = -std=c++17 -Wall

# -----------------------------------------------------------------------------
# DO NOT MODIFY BELOW (Links the standard Webots Makefile)
# -----------------------------------------------------------------------------
ifndef WEBOTS_HOME
os := $(shell uname -s)
ifeq ($(os), Linux)
 WEBOTS_HOME ?= /usr/local/webots
endif
endif

include $(WEBOTS_HOME)/resources/Makefile.include
```

---

## 4. Code Guide: C++ Controller Template

Here is a standard template for a Webots C++ controller that compiles cleanly with the Makefile above. Save this as `my_controller.cpp`.

```cpp
#include <webots/Robot.hpp>
#include <webots/Motor.hpp>
#include <webots/DistanceSensor.hpp>
#include <iostream>

using namespace webots;

int main(int argc, char **argv) {
    // 1. Initialize the Robot instance
    Robot *robot = new Robot();

    // 2. Get the basic time step of the current world
    int timeStep = (int)robot->getBasicTimeStep();

    // 3. Initialize devices (Ensure names match your Webots Robot node exactly)
    Motor *leftMotor = robot->getMotor("left_motor");
    Motor *rightMotor = robot->getMotor("right_motor");
    
    // Set motors to velocity control mode
    leftMotor->setPosition(INFINITY);
    rightMotor->setPosition(INFINITY);
    leftMotor->setVelocity(0.0);
    rightMotor->setVelocity(0.0);

    std::cout << "Ubuntu Webots Controller Started." << std::endl;

    // 4. Main Simulation Loop
    // wb_robot_step returns -1 when Webots terminates the controller
    while (robot->step(timeStep) != -1) {
        
        // Example: Set motor speeds
        double leftSpeed = 2.5;
        double rightSpeed = 2.5;

        leftMotor->setVelocity(leftSpeed);
        rightMotor->setVelocity(rightSpeed);
        
        // Add sensor reading and logic here
    }

    // 5. Clean up
    delete robot;
    return 0;
}
```