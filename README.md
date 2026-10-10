# Outrider

A modular simulation of a car-like robot, built on **ROS 2**, for evaluating and comparing algorithms across the whole robotics stack: controllers, planners, sensing, preprocessing, and machine-learning vision and language models.

The reference scenario is an **inspection and data-collection task** carried out in varied environments, where the robot has to adapt to changing terrain, lighting, obstacles, and mission requirements.

> **Status:** early development. The architecture and scope below describe the project's intent; sections marked *planned* are not implemented yet.

## Goals

- **Modularity first.** Every robot capability is a replaceable ROS 2 component with a well-defined interface. Swapping a planner, a controller, or a perception model should not require touching the rest of the system.
- **Fair benchmarking.** Run different algorithms under identical scenarios, seeds, and metrics, so results are comparable and reproducible.
- **Adaptability.** Evaluate how well each component copes when the environment changes, not only how it performs in a single fixed world.
- **Inspection Task.** Use inspection and data collection as the common benchmark, so improvements in a single module can be judged by their effect on the mission.

## Architecture

Outrider follows the standard ROS 2 node-and-topic model. Each stage of the pipeline is an independent package that talks to its neighbours only through ROS 2 interfaces (topics, services, actions), so implementations can be exchanged through launch files and parameters.

Design principles:

- **Interfaces over implementations.** To ensure modularity, the robot components must agree on message types and contracts, not on internals.
- **Configuration-driven.** Choosing which algorithm runs in each slot is done through launch arguments and parameter files.
- **Reproducible experiments.** Scenarios, random seeds, and configurations are versioned so a run can be repeated.
- **Metrics built in.** Tracking error, collision and safety events, planning time, inference latency, mission completion, and data quality are recorded for every run.

## The inspection task

The benchmark mission is to drive through an environment, visit inspection points, and collect data (images, point clouds, sensor readings) of sufficient quality. Environments are meant to vary along axes that stress adaptability:

- Terrain and surface conditions
- Lighting and weather
- Static and dynamic obstacles
- Layout and scale, from structured to unstructured areas
- Sensor degradation or dropout
- Interest object characteristics

A good result means finishing the inspection attending to the quantitative validation benchmarks regarding data quality, computational efficiency, speed, and integrity of the robot.

## Repository layout

*Planned.* This will be refined as the packages are added.

```
outrider/
├── description/      # Robot model (URDF/Xacro), sensors, vehicle parameters
├── simulation/       # Worlds, scenarios, and simulator bring-up
├── sensing/          # Sensor drivers and simulated sensor models
├── preprocessing/    # Filtering, calibration, fusion
├── perception/       # Detection, segmentation, mapping, localization
├── ml/               # Vision, vision-language, and language model integrations
├── planning/         # Global, local, and inspection planners
├── control/          # Controllers for car-like kinematics
├── mission/          # Inspection task logic and data collection
├── evaluation/       # Metrics, experiment runner, reports
└── bringup/          # Launch files and configuration for full-system runs
```

## Getting started

*Planned.* Build and run instructions will be added once the first packages land. The expected workflow is the usual ROS 2 one:

```bash
cd ~/ros2_ws/src
git clone <repository-url> outrider

cd ~/ros2_ws
colcon build --symlink-install
source install/setup.bash

ros2 launch outrider_bringup simulation.launch.py
```

## Contributing

Contributions are welcome, to get detailed information about development standards and guidelines check [CONTRIBUTING.md](CONTRIBUTING.md).

## License

See [LICENSE](LICENSE).
