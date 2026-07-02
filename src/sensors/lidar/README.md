# sensors_lidar

## Purpose

LiDAR drivers, point cloud preprocessing, calibration, and obstacle point cloud publication.

## Layer

`sensors/lidar`

## ROS 2 Design

This package is designed as a modular ROS 2 component with DDS-based communication.

Recommended interfaces:

- Topics for streaming data.
- Services for configuration and operator-triggered commands.
- Actions for long-running workflows.

## Standard Folders

```text
lidar/
├── CMakeLists.txt
├── package.xml
├── README.md
├── config/
├── launch/
├── src/
├── include/
└── test/
```

## Certification Note

Document whether this package is deterministic, AI-based, evidence-supporting, or external/vendor-dependent.

## Notes

Keep this package loosely coupled, independently testable, and reusable across simulation, SIL, HIL, VIL, and drone testing.
