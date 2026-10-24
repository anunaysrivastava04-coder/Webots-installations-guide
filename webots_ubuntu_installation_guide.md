# Webots Installation Guide for Ubuntu

Webots is a powerful, open-source 3D robot simulator. Depending on your needs, you can install it using Ubuntu's package managers (Snap/APT) for a quick setup, or compile it from source using `make` if you need to modify the simulator's core.

## Official Resources & Links
* **Official Website:** [cyberbotics.com](https://cyberbotics.com/)
* **Webots GitHub Repository:** [github.com/cyberbotics/webots](https://github.com/cyberbotics/webots)
* **Official Linux Installation Docs:** [Webots User Guide - Linux](https://cyberbotics.com/doc/guide/installation-procedure#linux-installation)
* **Source Compilation Docs:** [Webots Development - Linux](https://github.com/cyberbotics/webots/wiki/Linux-installation)

---

## Method 1: Building from Source (The `make` Method)
If you need to compile Webots from source (often required for advanced development or custom modifications), follow these steps. Webots uses a standard `Makefile` system.

### 1. Install Git and Build Dependencies
First, install git and clone the repository with all submodules:
```bash
sudo apt update
sudo apt install git
git clone --recurse-submodules https://github.com/cyberbotics/webots.git
cd webots
```

### 2. Install Compilation Dependencies
Webots provides a handy script to automatically install all required development packages (like Qt, physics engines, and graphics libraries) for Ubuntu:
```bash
sudo bash scripts/install/linux_compilation_dependencies.sh
```

### 3. Compile using `make`
Once the dependencies are installed, you can build Webots using the provided `Makefile`. To speed up the process, use the `-j` flag followed by the number of CPU cores you have (e.g., `-j8` for 8 cores):
```bash
make -j$(nproc)
```
*Note: This process may take a while depending on your system's hardware.*

### 4. Launch Webots
After the `make` command finishes successfully, you can run Webots directly from the compilation folder:
```bash
./webots
```

---

## Method 2: Quick Install via Snap (Recommended for standard users)
Snap is the easiest way to install Webots on Ubuntu as it bundles most dependencies automatically.

```bash
sudo apt update
sudo apt install snapd
sudo snap install webots
```
To launch it, simply type `webots` in your terminal or find it in your application menu.

---

## Method 3: Install via APT Repository
If you prefer standard Debian packages over Snap, you can add the official Cyberbotics repository.

### 1. Add the Cyberbotics GPG key
```bash
sudo mkdir -p /etc/apt/keyrings
wget -qO- https://cyberbotics.com/Cyberbotics.asc | sudo tee /etc/apt/keyrings/Cyberbotics.asc > /dev/null
```

### 2. Add the Repository
```bash
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/Cyberbotics.asc] https://cyberbotics.com/debian/ binary-amd64/" | sudo tee /etc/apt/sources.list.d/Cyberbotics.list
```

### 3. Update and Install
```bash
sudo apt update
sudo apt install webots
```

---

## Method 4: Portable Tarball
If you don't want to install Webots system-wide, you can download the pre-compiled `.tar.bz2` archive.

1. Go to the [Webots GitHub Releases page](https://github.com/cyberbotics/webots/releases).
2. Download the `webots-R202X.X-x86_64-ubuntu-XX.XX.tar.bz2` file matching your Ubuntu version.
3. Extract it to your preferred directory:
```bash
tar xjf webots-*.tar.bz2
```
4. Run it:
```bash
cd webots
./webots
```