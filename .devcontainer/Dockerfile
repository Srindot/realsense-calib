FROM ghcr.io/rtarun1/noetic:v1.0

# Add Labels
LABEL org.opencontainers.image.description="Image for Realsense camera calibration using tartan calib"
LABEL org.opencontainers.image.title="tartanCalib"
LABEL org.opencontainers.image.vendor="Srindot"
LABEL maintainer="bsdhatre@gmail.com"
LABEL org.opencontainers.image.licenses="MIT"

# Set default shell
SHELL ["/bin/bash", "-c"]

# Install Basic deps
RUN sudo apt-get update \
    && sudo apt-get -y install --no-install-recommends\
    libssl-dev \
    libusb-1.0-0-dev \
    libudev-dev \
    pkg-config \
    libgtk-3-dev \
    wget \
    cmake \
    build-essential \
    libglfw3-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    at \
    libaria-dev \
    v4l-utils \
    xterm \
    autoconf automake nano \
    python3-dev python3-pip python3-scipy python3-matplotlib \
    ipython3 python3-wxgtk4.0 python3-tk python3-igraph \
    libeigen3-dev libboost-all-dev libsuitesparse-dev \
    doxygen \
    libopencv-dev \
    libglew-dev \
    libpoco-dev libtbb-dev libblas-dev liblapack-dev libv4l-dev \
    python3-catkin-tools python3-osrf-pycommon \
    && sudo apt-get autoremove -y \
    && sudo apt-get clean -y \
    && sudo rm -rf /var/lib/apt/lists/*

# Build and install Realsense-SDK
WORKDIR /home/container_user
RUN git clone --branch v2.56.2 https://github.com/IntelRealSense/librealsense.git
WORKDIR /home/container_user/librealsense
RUN sudo mkdir -p /etc/udev/rules.d && \
    ./scripts/setup_udev_rules.sh \
    ./scripts/patch-realsense-ubuntu-lts-hwe.sh 
    
RUN mkdir build && cd build \
    && cmake ../ \
    && sudo make uninstall && make clean && make && sudo make install 

# Install ROS realsense2_camera package and dependencies
RUN sudo apt-get update && \
    sudo apt-get install -y \
    ros-noetic-realsense2-camera \
    ros-noetic-ddynamic-reconfigure \
    && sudo rm -rf /var/lib/apt/lists/*

RUN pip install dill


WORKDIR /home/container_user/catkin_ws/src/
RUN git clone https://github.com/castacks/tartancalib.git 

WORKDIR /home/container_user/catkin_ws
RUN source /opt/ros/noetic/setup.bash && \
    sudo catkin init && \
    sudo catkin config --extend /opt/ros/noetic && \
    sudo catkin config --merge-devel && \
    sudo catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release && \
    sudo catkin build

# Set default shell
SHELL ["/bin/bash", "-c"]


    
