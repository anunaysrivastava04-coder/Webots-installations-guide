# Webots-installations-guide
Webots is a powerful, open-source 3D robot simulator. Depending on your needs, you can install it using Ubuntu's package managers (Snap/APT) for a quick setup, or compile it from source using make if you need to modify the simulator's core.


Webots Ubuntu Project Template 🤖

This repository provides a standard template for developing, compiling, and running C/C++ robot controllers in Webots on an Ubuntu environment.

It utilizes standard GNU Makefiles for compilation and provides easy-to-use terminal commands for running simulations in various modes (real-time, headless, etc.).

📂 Project Structure

Webots relies on a specific directory structure to link worlds and controllers correctly:

my_webots_project/
├── README.md               # This documentation file
├── worlds/                 
│   └── my_world.wbt        # The Webots 3D environment file
└── controllers/
    └── my_controller/
        ├── my_controller.cpp # The C/C++ controller code
        └── Makefile          # Makefile to compile the controller


🛠️ Prerequisites

Before compiling or running this project, ensure you have the following installed on your Ubuntu system:

Webots: Installed via Snap, APT, or compiled from source.

Build Tools: GCC/G++ and Make.

sudo apt update
sudo apt install build-essential


⚙️ Compiling the Controller

This project uses the built-in Webots Makefile system. To compile the C/C++ controller, navigate to the controller directory and run make.

# Navigate to the controller directory
cd controllers/my_controller/

# Compile the code
make

# (Optional) Clean up build files
make clean

# (Optional) Compile for debugging (using gdb)
make debug


Note: Ensure your WEBOTS_HOME environment variable is set (e.g., export WEBOTS_HOME=/usr/local/webots) if it is not automatically detected by the Makefile.

🚀 Running the Simulation

You can launch the simulation directly from the Ubuntu terminal. Navigate to the root of this project and use the following commands:

1. Standard Launch (GUI with Real-time execution):

webots --mode=realtime worlds/my_world.wbt


2. Fast Mode (Maximum speed, skipping some render frames):

webots --mode=fast worlds/my_world.wbt


3. Headless Mode (No GUI - Best for Machine Learning & CI/CD):

webots --no-rendering worlds/my_world.wbt


4. Minimized Mode (Runs in the background):

webots --minimize worlds/my_world.wbt


📚 Further Documentation

Webots User Guide

Webots C/C++ API Reference
