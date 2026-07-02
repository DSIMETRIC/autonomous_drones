# Autonomous Drone Platform - Repository

## Overview

This repository contains the software stack for an intelligent autonomous drone platform built on ROS 2, DDS, Space ROS concepts, MAVLink, and a modular perception architecture inspired by the Perceptual Element Ensemble Pipeline (Peep).

The platform is designed to support:

- Small drones
- Medium multirotors
- Large UAV platforms
- Digital Twin environments
- GPS and GNSS-denied operations
- Multi-sensor perception
- AI-assisted mission execution
- Autonomous target tracking and target locking
- Collision avoidance
- Visual navigation
- Certification-oriented development

The architecture follows a distributed ROS 2 node-based approach where every subsystem is implemented as a loosely coupled package communicating through DDS topics, services, and actions.

---

# Design Principles

## Modular

Each capability is isolated in independent ROS 2 packages.

## Scalable

Supports multiple sensors, cameras, and vehicle types.

## Reusable

Nodes can be reused across simulation and real platforms.

## Testable

All packages support MIL, SIL, HIL, VIL, and flight testing.

## Certification Ready

Safety-critical components are separated from AI and experimental modules.

---

# Repository Structure

```text
ros2_autonomy_ws/

├── README.md
├── docs/
│   ├── architecture_overview.md
│   ├── certification_roadmap.md
│   ├── code_standards.md
│   ├── add_new_package_template.md
│   ├── development_roadmap.md
│   ├── validation_strategy.md
│   ├── technology_stack.md
│   ├── package_review_checklist.md
│   ├── perception_design.md
│   ├── tracking_design.md
│   ├── navigation_design.md
│   ├── sensor_fusion_design.md
│   ├── decision_design.md
│   ├── health_monitor_design.md
│   ├── digital_twin_strategy.md
│   ├── rosbag_strategy.md
│   └── safety_case_strategy.md
│
├── src/
│   ├── sensors/
│   │   ├── cameras/
│   │   ├── lidar/
│   │   ├── radar/
│   │   ├── gps/
│   │   └── imu/
│   │
│   ├── core_packages/
│   │   ├── perception/
│   │   ├── tracking/
│   │   ├── navigation/
│   │   ├── sensor_fusion/
│   │   ├── decision/
│   │   └── health_monitor/
│   │
│   ├── simulation/
│   │   ├── digital_twin/
│   │   ├── rosbag_database/
│   │   └── validation_tools/
│   │
│   └── external/
│       ├── px4_msgs/
│       ├── mavros/
│       ├── realsense_ros/
│       ├── velodyne_driver/
│       ├── ouster_ros/
│       ├── radar_sdk/
│       └── unreal_ros_bridge/
│
├── tools/
├── scripts/
├── docker/
├── datasets/
├── tests/
└── ci/
```

---

# Sensors Layer

## cameras

Provides drivers and interfaces for:

- RGB cameras
- EO cameras
- IR cameras
- Thermal cameras
- Zoom cameras

Outputs:

- `sensor_msgs/msg/Image`
- `sensor_msgs/msg/CameraInfo`

## lidar

Provides:

- 3D point clouds
- Obstacle detection
- Terrain reconstruction
- Mapping support

Outputs:

- `sensor_msgs/msg/PointCloud2`

## radar

Provides:

- Long-range detection
- Moving target indication
- All-weather sensing

Outputs:

- Radar tracks
- Radar detections

## gps

Provides:

- GNSS position
- RTK support
- Time synchronization

## imu

Provides:

- Accelerometer
- Gyroscope
- Orientation estimates

---

# Core Packages

## perception

Purpose:

Convert sensor information into a unified scene understanding.

Contains AI nodes:

### object_detection_node

YOLO / TensorRT.

### segmentation_node

Semantic segmentation.

### classification_node

Object classification.

### scene_graph_builder

Creates standardized scene graph representation.

### llm_visual_description_node

Generates natural language descriptions.

Example:

> Two vehicles moving north. One person standing near building.

Outputs:

- Detections
- Classifications
- Scene Graph
- Visual Reports

## tracking

Purpose:

Track detected objects over time and support target-oriented monitoring.

Modules:

- DeepSORT
- ByteTrack
- Tracklet Manager
- Multi-Camera Association
- Manual target selection
- Automatic target candidate proposal
- Target lock monitoring
- Re-identification

Outputs:

- Track IDs
- Velocities
- Predictions
- Target lock status
- Confidence scores

## navigation

Purpose:

Provide GPS-enabled navigation, visual navigation coordination, GNSS-denied navigation support, collision avoidance, waypoint management, and safe route execution.

Features:

- Waypoints
- Route planning
- RTL
- Geofencing
- Visual Odometry
- VIO
- SLAM
- Landmark Navigation
- Collision avoidance
- Safe trajectory updates

Sensors:

- Cameras
- LiDAR
- IMU
- GPS

## sensor_fusion

Purpose:

Fuse multi-modal sensor information into a common world model.

Inputs:

- Camera detections
- LiDAR point clouds
- Radar tracks
- GPS
- IMU
- Navigation state

Outputs:

- Fused object state
- Fused vehicle state
- Time-aligned sensor messages
- Spatially associated detections
- Unified world model

## decision

Purpose:

Mission execution engine and deterministic decision layer.

Capabilities:

- Task planning
- Route generation
- Dynamic re-planning
- Objective prioritization
- Behavior management
- Decision validation

## health_monitor

Purpose:

Safety-critical subsystem and certification readiness monitor.

Responsibilities:

- Flight envelope monitoring
- Battery monitoring
- Sensor health
- Node watchdogs
- Communication monitoring
- Emergency landing
- Return to base
- Requirements traceability
- Test coverage
- SIL/HIL status
- Flight readiness

---

# Simulation

## digital_twin

Unreal Engine integration.

Provides:

- Vehicle simulation
- Sensor simulation
- Mission rehearsal
- Scenario playback
- Synthetic data generation

## rosbag_database

Stores:

- Flight logs
- Perception datasets
- Tracking datasets
- Validation scenarios

Used for:

- Regression testing
- Model retraining
- Certification evidence

## validation_tools

Supports:

### MIL

Model In Loop.

### SIL

Software In Loop.

### HIL

Hardware In Loop.

### VIL

Vehicle In Loop.

### Flight Validation

Real platform testing.

---

# External Dependencies

External ROS packages and SDKs:

- PX4
- MAVROS
- RealSense
- Velodyne
- Ouster
- Radar SDK
- Unreal ROS Bridge

---

# Development Roadmap

## Phase 1

Target Locking and Tracking.

Duration: 4 weeks.

- Week 1: Prototype target selection, AI candidate proposal, and tracklet window.
- Week 2: Initial implementation, multi-camera handoff, and unit tests.
- Week 3: SIL testing with simulated feeds and rosbag replay.
- Week 4: Drone integration readiness package.

## Phase 2

Collision Avoidance and Navigation.

Duration: 3 weeks.

## Phase 3

Visual Navigation and GNSS-Denied Navigation.

Duration: 3 weeks.

## Phase 4

Mission Planning and Decision Validation.

Duration: 3 weeks.

---

# Validation Strategy

1. Rosbag Dataset Creation
2. Digital Twin Validation
3. MIL Testing
4. SIL Testing
5. HIL Testing
6. Small Drone Testing
7. Medium Drone Testing
8. Large UAV Testing
9. Certification Evidence Collection

---

# Technology Stack

- ROS 2
- Space ROS
- DDS
- MAVLink
- PX4
- ArduPilot
- OpenCV
- TensorRT
- CUDA
- PyTorch
- TensorFlow
- Unreal Engine
- Rosbag2
- Docker
- Kubernetes

---

# Mission Statement

Build a reusable, modular, scalable autonomous drone platform capable of perception, tracking, target locking, visual navigation, collision avoidance, sensor fusion, decision-making, health monitoring, simulation, validation, and intelligent mission execution across multiple vehicle classes.
