FROM ros:melodic-robot

# Update
RUN apt update \
 && apt-get install software-properties-common apt-utils -y

# Set working directory
WORKDIR /home/ros/src

# Get the realsense package from git
RUN git clone https://github.com/IntelRealSense/realsense-ros.git \
 && git clone https://github.com/appliedAI-Initiative/orb_slam_2_ros.git

# Set up Melodic keys
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 \ 
# Set up realsense keys
 && sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE \
 && sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE

#Add realsense repo
RUN sudo add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u \
# Install required realsense and ROS packages
 && apt-get update \
 && apt-get install \
    librealsense2-dkms \
    librealsense2-utils \
    librealsense2-dev \
    librealsense2-dbg \
    ros-melodic-tf2-geometry-msgs \
    python-catkin-tools -y

WORKDIR /home/ros

# Install ROS dependencies
RUN rosdep update \
 && rosdep install --from-paths src --ignore-src -r -y --skip-keys=librealsense2

# build ros package source
RUN catkin config --extend /opt/ros/$ROS_DISTRO \
 && catkin build \
 && echo "source /home/ros/devel/setup.bash" >> /$HOME/.bashrc

CMD "bash"