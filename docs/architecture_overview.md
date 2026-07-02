# Architecture Overview

The ROS 2 Autonomy Workspace separates the autonomy stack into four top-level domains:

- `sensors`
- `core_packages`
- `simulation`
- `external`

The `core_packages` folder contains the main autonomy logic:

- perception
- tracking
- navigation
- sensor_fusion
- decision
- health_monitor

This layout avoids mixing low-level sensor drivers with core autonomy logic and keeps third-party packages isolated under `external`.

## Core Package Mapping

| Original Concept | New Package |
|---|---|
| perception | perception |
| target_tracking | tracking |
| target_locking | tracking |
| visual_navigation | navigation |
| collision_avoidance | navigation |
| mission_planning | decision |
| safety_manager | health_monitor |
| certification_monitor | health_monitor |
| mavlink_bridge | external or navigation integration |
| llm_visual_description | perception |
| scene_graph_builder | perception |
| digital_twin | simulation |
| rosbag_database | simulation |
