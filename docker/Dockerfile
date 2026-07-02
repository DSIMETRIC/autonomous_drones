FROM ros:humble-ros-base

ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_WS=/ros2_ws

RUN apt-get update && apt-get install -y \
    python3-pip \
    python3-colcon-common-extensions \
    python3-opencv \
    ros-humble-cv-bridge \
    ros-humble-image-transport \
    ros-humble-sensor-msgs \
    ros-humble-std-msgs \
    ros-humble-geometry-msgs \
    ros-humble-foxglove-bridge \
    && rm -rf /var/lib/apt/lists/*

WORKDIR ${ROS_WS}
COPY . ${ROS_WS}
RUN /bin/bash -c "source /opt/ros/humble/setup.bash && colcon build --symlink-install"
CMD ["/bin/bash"]
