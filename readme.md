# Robot Simulation (RoboCup@Home 2026)

This repository is the **master workspace** for running our robot simulation in a Docker-based environment.
It contains multiple **submodule repositories** for different parts of the system: robot description, control, and Gazebo simulation.

---

## 📂 Repository Structure

```
robot_simulation/
├── docker/               # Docker setup for simulation
│   ├── compose.simulation.yml  # Docker Compose config
│   ├── Dockerfile.simulation   # Simulation container definition
│   ├── justfile               # Helper commands
│   └── readme.md
├── robot_control/        # Robot control packages (submodule)
├── robot_description/    # URDF, meshes, and configs (submodule)
├── robot_gazebo/         # Gazebo world, launch, and configs (submodule)
└── readme.md             # This file
```

---
## 🛠 Requirements

* **Git LFS**
* **Docker**
* **Docker Compose**
* (Optional) [`just`](https://github.com/casey/just) for helper commands

---

## 🚀 Getting Started

### Initialize git LFS

Install **git LFS**:

```bash
git lfs install
```

### Clone with Submodules

Since this repo uses **Git submodules**, make sure you clone it with:

```bash
git clone --recurse-submodules https://github.com/EIC-Robocup-2026/simulation.git
git lfs pull
```

If you already cloned without submodules:

```bash
git submodule update --init --recursive
git lfs pull
```

---
### Build & Run on your computer

```bash
# cd ros2_ws and clone this repo to src directory

# Install nesscessary package
rosdep install --from-paths src

# Build and run simulation
colcon build --symlink-install
source install/setup.bash
ros2 launch robot_simulation gzsim_omnibot.launch.py
```


### Build & Run in Docker

The simulation runs fully inside Docker.

**Build the simulation container:**

```bash
cd docker
sudo docker build -t "eic-robocup2026-development:latest" -f Dockerfile.simulation ..
```

**Run the simulation:**

```bash
# Allow the Docker container to connect to the X server.
# This command only needs to be run once after a system boot.
xhost local:root

# Run the simulation
sudo docker compose -f compose.simulation.yml up
```

---

###  Using `just` for Shortcuts (Optional)

If you have [`just`](https://github.com/casey/just) installed:
```bash
# enter docker directory
just build-sim        # Build simulation container
just run-sim          # Start simulation
just stop-sim         # Stop simulation
just shell $CONTAINER_NAME # enter container terminal
just run-topic-list   # List ROS2 Topics
```
---

## 📦 Submodules

| Submodule           | Purpose                                                                     |
| ------------------- | --------------------------------------------------------------------------- |
| `robot_control`     | ROS 2 packages for robot motion control (e.g., omni wheel drive controller) |
| `robot_description` | URDF models, meshes, and RViz/Gazebo configs                                |
| `robot_gazebo`      | Gazebo simulation environment, worlds, and launch files                     |

---

## 📌 Notes

* All simulation assets and ROS 2 packages are in the submodules.
* The Docker environment includes all dependencies, so you don’t need ROS installed locally.
* This setup is designed for **RoboCup@Home 2026 development**.

