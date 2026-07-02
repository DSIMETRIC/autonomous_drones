# ModalAI VOXL2 ROS 2 Target Follow Demo

Ready-to-run ROS 2 package for manual target selection and visual tracking on ModalAI VOXL2 using OpenCV tracking, ROS 2, Docker, and Foxglove.

## What This Package Provides

- ROS 2 package: `target_follow_demo`
- Node: `target_follow_node`
- Dockerfile
- Docker Compose deployment
- Foxglove bridge
- Manual target selection with OpenCV ROI
- Target tracking output image
- Target lock status topic
- Gimbal / servo command topic

## Architecture

```text
VOXL Camera
    |
    v
voxl-camera-server
    |
    v
voxl-mpa-to-ros2
    |
    v
/hires/image_raw or /tracking/image_raw
    |
    v
target_follow_node
    |
    +--> /camera/tracked_image
    +--> /target_lock/status
    +--> /camera_follow/cmd
    |
    v
PX4 / Gimbal / Servo Controller
```

## Topics

### Subscribed

| Topic | Type | Description |
|---|---|---|
| `/hires/image_raw` | `sensor_msgs/Image` | Default VOXL2 camera image topic |
| `/tracking/image_raw` | `sensor_msgs/Image` | Alternative VOXL2 tracking camera topic |

### Published

| Topic | Type | Description |
|---|---|---|
| `/camera/tracked_image` | `sensor_msgs/Image` | Camera image with tracking overlay |
| `/target_lock/status` | `std_msgs/String` | `WAITING_FOR_TARGET`, `LOCKED`, `TRACKING`, `LOST` |
| `/camera_follow/cmd` | `geometry_msgs/Twist` | Pan/tilt command output |

## Prerequisites on VOXL2

Check VOXL services:

```bash
voxl-inspect-services
voxl-inspect-cam
```

Install and run the VOXL ROS bridge if needed:

```bash
apt-get update
apt-get install -y voxl-mpa-to-ros2
source /opt/ros/foxy/setup.bash
ros2 run voxl_mpa_to_ros2 voxl_mpa_to_ros2
```

Verify image topics:

```bash
ros2 topic list
ros2 topic hz /hires/image_raw
```

## Build and Run with Docker Compose

From the repository root:

```bash
xhost +local:docker
docker compose up --build
```

The compose stack starts:

- `foxglove_bridge`
- `target_tracker`

## Choose Camera Topic

Default:

```bash
docker compose up --build
```

Use tracking camera instead:

```bash
IMAGE_TOPIC=/tracking/image_raw docker compose up --build
```

Disable OpenCV display window:

```bash
DISPLAY_WINDOW=false docker compose up --build
```

## Select a Target

When the OpenCV window appears:

1. Press `ENTER` or `s` to start selection.
2. Drag a box around the target.
3. Press `ENTER` to confirm.
4. The tracker publishes status and commands.

Controls:

```text
ENTER or s  Start target selection
r           Reselect target
q           Quit
```

## Foxglove

Foxglove bridge listens on port `8765`.

Connect from Foxglove Desktop:

```text
ws://VOXL_IP:8765
```

Recommended panels:

| Panel | Topic |
|---|---|
| Image | `/camera/tracked_image` |
| Raw Messages | `/target_lock/status` |
| Raw Messages | `/camera_follow/cmd` |

## Gimbal / Servo Mapping

The node publishes `geometry_msgs/Twist` on `/camera_follow/cmd`.

```text
angular.z -> pan
angular.y -> tilt
```

Connect this topic to:

- ModalAI gimbal node
- PX4 offboard controller
- Servo controller
- Custom flight logic

## Native ROS 2 Build

```bash
source /opt/ros/humble/setup.bash
colcon build --symlink-install
source install/setup.bash
ros2 run target_follow_demo target_follow_node --ros-args -p image_topic:=/hires/image_raw
```

## Future Upgrades

This package can later be extended with:

- YOLOv8
- YOLO-NAS
- TensorRT
- DeepSORT
- ByteTrack
- Object re-identification
- Multi-target tracking
- Visual navigation
- Sensor fusion

The ROS 2 topic interface stays the same, so the architecture can evolve without redesigning the whole autonomy stack.
