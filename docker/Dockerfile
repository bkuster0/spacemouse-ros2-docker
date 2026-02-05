FROM ros:humble-ros-base

RUN apt-get update && apt-get install -y \
    git \
    python3-colcon-common-extensions \
    udev \
    build-essential \
    cmake \ 
    cmake-curses-gui \
    software-properties-common
#    && rm -rf /var/lib/apt/lists/*

RUN add-apt-repository universe && apt update

# Create workspace
WORKDIR /ros2_ws/src

RUN git clone https://github.com/ros-drivers/joystick_drivers
RUN mv joystick_drivers/spacenav /ros2_ws/src
RUN rm -rf joystick_drivers
RUN rosdep install --from-paths spacenav -y

# Build
WORKDIR /ros2_ws
RUN . /opt/ros/humble/setup.sh && \
    colcon build --symlink-install

# Copy FastDDS config to disable shared memory and rely on UDP
COPY fastrtps_udp_only.xml /

# Source entrypoint
RUN echo "source /ros2_ws/install/setup.bash" >> /root/.bashrc

