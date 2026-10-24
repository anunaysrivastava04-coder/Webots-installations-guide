# Webots Controller Programming Guide (Makefile Focus)

When writing C or C++ controllers for your robots in Webots on Ubuntu, the standard and most efficient way to compile them is by using `make`. Webots provides a built-in `Makefile.include` that handles all the complex linking to the Webots libraries automatically.

## Official Resources & API Links

*   **Webots C API Reference:** [cyberbotics.com/doc/reference/c-api](https://cyberbotics.com/doc/reference/c-api)
*   **Webots C++ API Reference:** [cyberbotics.com/doc/reference/cpp-api](https://cyberbotics.com/doc/reference/cpp-api)
*   **Controller Programming Guide:** [cyberbotics.com/doc/guide/controller-programming](https://cyberbotics.com/doc/guide/controller-programming)

---

## 1. Directory Structure

Webots expects your controller code to be organized in a specific way inside your Webots project folder. For a controller named `my_controller`, the structure MUST look like this:

```text
my_webots_project/
└── controllers/
    └── my_controller/
        ├── my_controller.c    (or .cpp)
        └── Makefile
```

---

## 2. The Webots Makefile

You do not need to write a complex Makefile from scratch. You only need a few lines that point to the main Webots `Makefile.include` file. 

Create a file named `Makefile` inside your `my_controller` folder and add the following code:

```makefile
# -----------------------------------------------------------------------------
# Standard Webots Makefile for C/C++ Controllers
# -----------------------------------------------------------------------------

# You can set the compiler here if needed (gcc for C, g++ for C++)
# CC = gcc
# CXX = g++

# Optional: Add custom C/C++ flags here
CFLAGS = -Wall 
CXXFLAGS = -Wall

# -----------------------------------------------------------------------------
# REQUIRED: Include the Webots standard Makefile
# -----------------------------------------------------------------------------
# WEBOTS_HOME is usually set automatically by Webots. 
# If compiling from an external terminal, ensure WEBOTS_HOME is set in your environment 
# (e.g., export WEBOTS_HOME=/usr/local/webots or export WEBOTS_HOME=/snap/webots/current/usr/share/webots)

ifndef WEBOTS_HOME
os := $(shell uname -s)
ifeq ($(os), Linux)
 WEBOTS_HOME ?= /usr/local/webots
endif
endif

include $(WEBOTS_HOME)/resources/Makefile.include
```

---

## 3. Example C Controller Code

Create a file named `my_controller.c` in the same directory as the Makefile:

```c
#include <webots/robot.h>
#include <webots/motor.h>
#include <stdio.h>

#define TIME_STEP 64

int main(int argc, char **argv) {
  // Initialize the Webots API
  wb_robot_init();

  printf("Hello from Webots C Controller on Ubuntu!\n");

  // Get a motor device (assuming a motor named "motor1" exists on the robot)
  // WbDeviceTag motor = wb_robot_get_device("motor1");
  // wb_motor_set_position(motor, INFINITY);
  // wb_motor_set_velocity(motor, 1.0);

  // Main control loop
  while (wb_robot_step(TIME_STEP) != -1) {
    // Read sensors, process data, and send motor commands here
  }

  // Cleanup Webots API
  wb_robot_cleanup();
  return 0;
}
```

---

## 4. How to Compile in Ubuntu

You can compile this code in two different ways on Ubuntu:

### Method A: Compiling from inside the Webots IDE
1. Open your project in the Webots application.
2. Open `my_controller.c` in the built-in Webots text editor.
3. Click the **Build** (gear icon) button in the editor toolbar. Webots will automatically run the `Makefile` and link everything perfectly.

### Method B: Compiling from the Ubuntu Terminal
If you prefer using an external editor (like VS Code or Vim) and compiling via the command line:

1. Open your Ubuntu terminal.
2. Navigate to your controller folder:
   ```bash
   cd /path/to/my_webots_project/controllers/my_controller/
   ```
3. Set your `WEBOTS_HOME` path if it isn't set globally. For a standard `.deb` or `apt` installation, it is usually `/usr/local/webots`. For snap, it is different:
   ```bash
   export WEBOTS_HOME=/usr/local/webots 
   ```
4. Run the `make` command:
   ```bash
   make
   ```
   
If successful, this will generate an executable named `my_controller` which Webots will automatically run when you start your simulation. To clean up the build files, simply run:
```bash
make clean
```