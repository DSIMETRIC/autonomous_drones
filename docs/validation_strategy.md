# Validation Strategy

## Pipeline

```text
MIL
 ↓
SIL
 ↓
Digital Twin
 ↓
Rosbag Replay
 ↓
HIL
 ↓
Small Drone
 ↓
Medium Drone
 ↓
Large UAV
 ↓
Certification Evidence
```

## Rosbag Validation

The rosbag database stores:

- flight logs
- perception datasets
- tracking datasets
- validation scenarios

Use cases:

- regression testing
- model retraining
- certification evidence
