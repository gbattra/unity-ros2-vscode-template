FROM ros:foxy-ros-base

ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Make ROS2 Workspace Dirss
RUN mkdir -p /home/dev_ws/src

# Reference script with commands to source workspace
COPY ./docker/source_ros.sh /home/source_ros.sh

# Build the workspace
RUN cd /home/dev_ws && . /opt/ros/$ROS_DISTRO/setup.sh && colcon build

# Source the workspace on sign in
RUN echo ". /home/source_ros.sh" >> /home/$USERNAME/.bashrc

USER $USERNAME