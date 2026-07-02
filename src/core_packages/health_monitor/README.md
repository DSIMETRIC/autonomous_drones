# core_packages_health_monitor

## Purpose

Vehicle health, subsystem diagnostics, watchdogs, fail-safe state monitoring, certification readiness, and operational readiness status.

## Layer

`core_packages/health_monitor`

## ROS 2 Design

This package is designed as a modular ROS 2 component with DDS-based communication.

Recommended interfaces:

- Topics for streaming data.
- Services for configuration and operator-triggered commands.
- Actions for long-running workflows.

## Standard Folders

```text
health_monitor/
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
