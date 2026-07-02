# Code Standards and Package Contribution Guide

## Purpose

This document defines the minimum standard for adding or modifying ROS 2 packages in this repository.

## Package Naming

Use lowercase snake_case.

Good:

- `core_packages/perception`
- `core_packages/sensor_fusion`
- `sensors/cameras`

Avoid:

- `CorePackages`
- `SensorFusionPackage`
- `my_new_test_pkg`
- names with company names, personal names, or temporary labels

## Required Files for Every Package

Every package must include:

```text
package.xml
CMakeLists.txt
README.md
config/
launch/
src/
include/
test/
```

## Package README Requirements

Each package README must include:

- Purpose
- Layer
- Main nodes
- Published topics
- Subscribed topics
- Services
- Actions
- Parameters
- Launch files
- Test strategy
- Known limitations
- Certification status if applicable

## Node Naming

Use descriptive names ending in `_node`.

Good:

- `object_detection_node`
- `multi_object_tracker_node`
- `sensor_fusion_node`
- `mission_decision_engine_node`

Avoid:

- `node1`
- `test_node`
- `main`
- `temp_tracker`

## Topic Naming

Use stable, namespaced topics.

Recommended pattern:

```text
/<domain>/<package>/<signal>
```

Examples:

```text
/sensors/cameras/image_raw
/perception/detections
/tracking/tracklets
/sensor_fusion/fused_objects
/navigation/state
/decision/mission_state
/health/system_status
```

Avoid:

```text
/image
/data
/output
/test
/tmp
```

## Parameters

All nodes should expose parameters through YAML files under `config/`.

Required baseline parameters:

```yaml
enabled: true
update_rate_hz: 30.0
frame_id: base_link
use_sim_time: false
```

## Testing Requirements

Minimum tests:

- Unit tests for core logic.
- Launch tests for startup.
- Parameter validation tests.
- Rosbag replay tests for perception, tracking, fusion, and navigation.
- SIL tests for decision and navigation behavior.

## Certification-Aware Rules

For packages with certification potential:

Avoid:

- unbounded memory allocation in real-time paths
- nondeterministic execution
- hidden global state
- uncontrolled threads
- dynamic plugin loading in safety-critical paths
- AI model inference inside deterministic control nodes

Required:

- traceable requirements
- deterministic state machines
- bounded runtime behavior
- explicit failure modes
- test coverage reports
- reviewable configuration

## AI Package Rules

AI packages must document:

- model name
- model version
- dataset version
- input format
- output format
- confidence threshold
- known limitations
- validation dataset
- fallback behavior

AI outputs should be treated as recommendations unless validated by deterministic safety logic.
