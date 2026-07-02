# Dsimetric Space ROS Autonomy Workspace

## Overview

This repository defines a modular Space ROS autonomy workspace for an intelligent drone platform. It is organized around separated sensor drivers, perception services, target tracking, target locking for monitoring and identification, navigation, visual navigation, collision avoidance, mission planning, safety management, certification monitoring, simulation, and external integrations.

The architecture is inspired by a Perceptual Element Ensemble Pipeline approach: perception capabilities are built as modular elements, each implemented as a ROS 2 node or package, communicating through standardized topics, services, and actions over DDS.

The design goal is to provide a reusable and scalable foundation that can be applied across small drones, medium drones, large UAVs, Digital Twin environments, and validation pipelines.

## Key Design Goals

- Modular base architecture for multiple vehicle classes and mission scenarios.
- Flexible framework where the core system can grow as project requirements evolve.
- Scalable framework for adding sensors, AI nodes, tracking modules, and mission services.
- Maintainable package layout with clear ownership by subsystem.
- Portable operator-facing tooling and reusable ground-control integrations.
- Easy data access for live quality control, offline replay, rosbag validation, and regression testing.
- ROS 2 / DDS communication model with loosely coupled nodes and secure message bus capability.
- Reusable perception elements that can be swapped across algorithms while preserving standardized outputs.
- Support for multi-camera fusion, sensor fusion, scene graph generation, target tracking, and visual AI descriptions.

## Repository Structure

```text
src/

├── sensors/
│   ├── cameras/
│   ├── lidar/
│   ├── radar/
│   ├── gps/
│   └── imu/

├── core_space_packages/
│   ├── perception/
│   ├── target_tracking/
│   ├── target_locking/
│   ├── navigation/
│   ├── visual_navigation/
│   ├── collision_avoidance/
│   ├── mission_planning/
│   ├── safety_manager/
│   ├── certification_monitor/
│   └── mavlink_bridge/

├── simulation/
│   ├── digital_twin/
│   ├── rosbag_database/
│   └── validation_tools/

└── external/
    ├── px4_msgs/
    ├── mavros/
    ├── realsense_ros/
    ├── velodyne_driver/
    ├── ouster_ros/
    ├── radar_sdk/
    └── unreal_ros_bridge/
```

## Architecture Concept

The software is a distributed framework of self-contained ROS 2 processes called nodes. Nodes discover each other and communicate at runtime using DDS. Each subsystem owns a specific portion of autonomy functionality, such as camera acquisition, AI detection, tracking, state estimation, mission planning, or safety monitoring.

Nodes communicate by publishing or subscribing to topics and by exposing services or actions for command-oriented behavior. This separates concerns while allowing the system to make complex decisions using shared, standardized information.

## Sensors Layer

### `src/sensors/cameras`

Camera acquisition and calibration packages.

Supported sensor types:

- RGB cameras
- EO cameras
- IR cameras
- Thermal cameras
- Stereo cameras
- Zoom cameras

Expected outputs:

- `sensor_msgs/msg/Image`
- `sensor_msgs/msg/CameraInfo`
- Rectified image topics
- Camera diagnostics

### `src/sensors/lidar`

LiDAR acquisition and point cloud preprocessing.

Responsibilities:

- LiDAR driver integration
- Point cloud filtering
- Ground segmentation
- Obstacle cloud generation
- Calibration support

Expected outputs:

- `sensor_msgs/msg/PointCloud2`
- Filtered point clouds
- Obstacle point clouds

### `src/sensors/radar`

Radar acquisition and radar object reporting.

Responsibilities:

- Radar SDK interface
- Moving object detection
- Range / range-rate extraction
- Radar track publication

Expected outputs:

- Radar detections
- Radar tracks
- Radar diagnostics

### `src/sensors/gps`

GNSS and positioning integration.

Responsibilities:

- GNSS data acquisition
- RTK support
- Time synchronization
- Position quality monitoring

### `src/sensors/imu`

Inertial measurement integration.

Responsibilities:

- Acceleration and angular velocity
- Orientation support
- Time alignment
- Sensor calibration

## Core Space Packages

### `src/core_space_packages/perception`

The perception package contains AI nodes and perception services that convert raw sensor data into actionable scene understanding.

Responsibilities:

- AI object detection
- Object classification
- Semantic segmentation
- Sensor fusion for perception
- Scene graph construction
- LLM visual description
- Standardized perception messages

Example nodes:

- `object_detection_node`
- `classification_node`
- `segmentation_node`
- `scene_graph_builder_node`
- `llm_visual_description_node`
- `perception_health_monitor_node`

Outputs:

- Object detections
- Object classes
- Scene graph
- Visual scene descriptions
- Perception confidence
- Perception diagnostics

### `src/core_space_packages/target_tracking`

Tracks detected objects over time and maintains persistent identities.

Responsibilities:

- Multi-object tracking
- Tracklet generation
- Track ID management
- Data association
- Trajectory prediction
- Multi-camera track continuity

Example nodes:

- `multi_object_tracker_node`
- `tracklet_manager_node`
- `data_association_node`
- `trajectory_prediction_node`

Outputs:

- Track IDs
- Track confidence
- Track state
- Velocity and heading
- Predicted trajectory

### `src/core_space_packages/target_locking`

First implementation module.

This package supports operator-selected or AI-suggested target locking for monitoring and identification workflows.

Workflow:

1. Operator selects a target or AI proposes a candidate.
2. System confirms the target using perception and tracking.
3. Tracklet window is initialized.
4. Target lock is maintained with confidence scoring.
5. Re-identification is performed if the target is partially occluded or changes camera view.
6. System provides lock status, track state, and confidence to the operator interface.

Example nodes:

- `target_selection_node`
- `auto_lock_candidate_node`
- `target_lock_manager_node`
- `tracklet_window_node`
- `reidentification_node`
- `lock_confidence_monitor_node`

Outputs:

- Target lock status
- Selected track ID
- Lock confidence
- Lock mode: manual or automatic
- Re-identification status

Initial implementation plan:

- Week 1: Prototype target selection, detection pipeline, and tracklet window.
- Week 2: Implement target locking, unit tests, and multi-camera support.
- Week 3: Software-in-the-loop testing with simulated camera feeds and rosbags.
- Week 4: Prepare deployment package for drone testing.

### `src/core_space_packages/navigation`

Global navigation and state estimation.

Responsibilities:

- Waypoint navigation
- Route following
- State estimation
- GNSS integration
- Return-to-base behavior
- Navigation health monitoring

### `src/core_space_packages/visual_navigation`

Visual and GNSS-denied navigation.

Responsibilities:

- Visual odometry
- Visual-inertial odometry
- SLAM
- Landmark localization
- Map matching
- Camera / LiDAR / IMU fusion

### `src/core_space_packages/collision_avoidance`

Obstacle detection and safe trajectory generation.

Responsibilities:

- Static obstacle avoidance
- Dynamic obstacle prediction
- Safe corridor generation
- Local trajectory planning
- Geofence and no-fly-zone checks

### `src/core_space_packages/mission_planning`

Mission planning and execution.

Responsibilities:

- Mission upload
- Behavior tree execution
- Task scheduling
- Objective prioritization
- Adaptive re-planning
- Mission state reporting

### `src/core_space_packages/safety_manager`

Safety-critical monitoring and failsafe logic.

Responsibilities:

- Vehicle health monitoring
- Battery monitoring
- Geofence enforcement
- Emergency landing
- Return-to-base trigger
- Safety interlocks

### `src/core_space_packages/certification_monitor`

Certification-readiness and assurance support.

Responsibilities:

- Requirements traceability
- Test coverage monitoring
- Validation evidence collection
- SIL/HIL status reporting
- Safety case evidence
- Certification artifact organization

### `src/core_space_packages/mavlink_bridge`

ROS 2 to MAVLink integration.

Responsibilities:

- PX4 / ArduPilot telemetry
- Command bridge
- Mission upload
- Vehicle state monitoring
- Autopilot health checks

## Simulation

### `src/simulation/digital_twin`

Digital Twin integration for simulation and validation.

Responsibilities:

- Simulated sensors
- Scenario playback
- Environment modeling
- Mission rehearsal
- Synthetic data generation

### `src/simulation/rosbag_database`

Rosbag2 dataset management.

Responsibilities:

- Dataset storage
- Replay tooling
- Scenario library
- Ground truth metadata
- Regression testing datasets

### `src/simulation/validation_tools`

Validation support tooling.

Responsibilities:

- Model-in-the-loop
- Software-in-the-loop
- Hardware-in-the-loop
- Vehicle-in-the-loop
- Flight test metrics

## External Integrations

### `src/external/px4_msgs`

PX4 message definitions.

### `src/external/mavros`

MAVROS integration.

### `src/external/realsense_ros`

Intel RealSense camera integration.

### `src/external/velodyne_driver`

Velodyne LiDAR integration.

### `src/external/ouster_ros`

Ouster LiDAR integration.

### `src/external/radar_sdk`

Radar vendor SDK wrapper.

### `src/external/unreal_ros_bridge`

Unreal Engine / ROS bridge.

## Build Instructions

```bash
source /opt/ros/humble/setup.bash
cd dsimetric_space_ros_autonomy_ws
colcon build --symlink-install
source install/setup.bash
```

## Suggested Development Workflow

```bash
# Build all packages
colcon build --symlink-install

# Build one package
colcon build --packages-select core_space_packages_target_locking

# Run tests
colcon test
colcon test-result --verbose

# Source workspace
source install/setup.bash
```

## Validation Workflow

1. Unit Tests
2. Rosbag Replay
3. Software-in-the-loop
4. Digital Twin
5. Hardware-in-the-loop
6. Drone Integration Testing
7. Flight Validation
8. Certification Evidence Collection

## Technology Stack

- ROS 2
- Space ROS concepts
- DDS
- MAVLink
- PX4 / ArduPilot
- OpenCV
- PyTorch
- TensorFlow
- TensorRT
- Unreal Engine
- Rosbag2
- Docker

## Mission Statement

Build a modular, reusable, scalable, and testable ROS 2 autonomy workspace for perception, tracking, target locking, navigation, collision avoidance, mission planning, safety monitoring, simulation, and validation across multiple drone platforms.
