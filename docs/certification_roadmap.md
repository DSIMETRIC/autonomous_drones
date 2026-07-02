# Certification Roadmap

## Purpose

This document identifies which packages are better candidates for certification readiness and which should remain isolated as experimental or non-certifiable modules.

The goal is not to claim certification, but to design the repository so safety-critical logic can be tested, traced, reviewed, and isolated from AI-heavy functions.

## Certification-Oriented Partition

```text
Perception / AI / Tracking Recommendation
        ↓
Decision Validator
        ↓
Health Monitor
        ↓
Navigation
        ↓
Autopilot Interface
```

## Package Certification Potential

| Package | Certification Potential | Rationale |
|---|---:|---|
| `core_packages/health_monitor` | High | Deterministic diagnostics, watchdogs, readiness checks, health states, certification evidence. |
| `core_packages/navigation` | High to Medium | Can be requirements-driven when algorithms are deterministic, bounded, and fully tested. |
| `core_packages/sensor_fusion` | Medium | EKF/time sync/fusion can be certifiable if deterministic and test-covered. |
| `core_packages/decision` | Medium to Partial | Behavior trees and mission rules may be certifiable if constrained and validated. |
| `sensors/gps` | Medium | Driver may be qualified if deterministic and traceable. |
| `sensors/imu` | Medium | Driver and calibration pipeline may be qualified with robust tests. |
| `sensors/lidar` | Low to Medium | Driver and preprocessing can be tested; vendor stack may limit evidence. |
| `sensors/radar` | Low to Medium | Vendor SDK and signal processing may limit evidence. |
| `sensors/cameras` | Low to Medium | Camera drivers can be tested; image interpretation is not certifiable by default. |
| `core_packages/tracking` | Low to Partial | Deterministic tracking may be testable; learned re-identification should be isolated. |
| `core_packages/perception` | Low | AI detection, segmentation, and visual descriptions are model/data dependent. |
| `simulation/digital_twin` | Evidence Support | Supports validation evidence but is not flight-critical runtime software. |
| `simulation/rosbag_database` | Evidence Support | Supports regression testing, traceability, and dataset replay. |
| `simulation/validation_tools` | Evidence Support | Supports MIL/SIL/HIL evidence and test repeatability. |
| `external/*` | Depends on vendor | Third-party package evidence depends on vendor artifacts and qualification data. |

## Recommended Certifiable Core

Start certification-oriented work with:

- `core_packages/health_monitor`
- `core_packages/navigation`
- deterministic parts of `core_packages/sensor_fusion`
- deterministic parts of `core_packages/decision`

## Recommended Non-Certifiable / Isolated Core

Keep these isolated from direct flight-critical authority:

- AI object detection
- semantic segmentation
- visual language model descriptions
- learned re-identification
- learned tracking components
- mission optimization AI

## Roadmap

### Phase 1 - Architecture Partitioning

- Separate AI outputs from safety-critical commands.
- Define stable messages between AI and deterministic domains.
- Add safety validation before any mission action.

### Phase 2 - Requirements and Traceability

- Add requirements IDs to package READMEs.
- Link requirements to tests.
- Define expected behavior and failure modes.

### Phase 3 - Test Evidence

- Unit tests
- Integration tests
- Rosbag replay tests
- SIL test scenarios
- HIL test scenarios

### Phase 4 - Determinism and Static Analysis

- Avoid unbounded execution.
- Avoid dynamic behavior in certifiable nodes.
- Add linting, static analysis, and code coverage.
- Track worst-case runtime.

### Phase 5 - Flight Readiness Evidence

- Generate readiness reports.
- Verify sensor health.
- Verify communication status.
- Verify fallback modes.
- Capture validation artifacts.
