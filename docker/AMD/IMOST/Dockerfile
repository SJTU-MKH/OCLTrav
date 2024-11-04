FROM markhui/docker-wvn_nvidia:wvn_ros2

# my proxy
ENV http_proxy=http://127.0.0.1:7890
ENV https_proxy=http://127.0.0.1:7890


# unitree go2 support
RUN apt update && apt install -y git cmake ros-foxy-rmw-cyclonedds-cpp ros-foxy-rosidl-generator-dds-idl ros-foxy-ros1-bridge
RUN pip install lark

RUN cd ~ && git clone https://github.com/unitreerobotics/unitree_ros2

RUN cd ~/unitree_ros2/cyclonedds_ws/src && \
    git clone https://github.com/ros2/rmw_cyclonedds -b foxy && \
    git clone https://github.com/eclipse-cyclonedds/cyclonedds -b releases/0.10.x

RUN /bin/bash -c "cd ~/unitree_ros2/cyclonedds_ws; colcon build --packages-select cyclonedds"

RUN /bin/bash -c "cd ~/unitree_ros2/cyclonedds_ws; source /opt/ros/foxy/setup.bash; colcon build"

# livox sdk
RUN mkdir -p /dep && cd /dep && \
    git clone https://github.com/Livox-SDK/Livox-SDK.git && \
    cd Livox-SDK && \
    cd build && cmake .. && \
    make -j16 && make install && \
    cd ../.. && \
    git clone https://github.com/Livox-SDK/Livox-SDK2.git && \
    cd Livox-SDK2 && \
    mkdir build && cd build && cmake .. && \
    make -j16 && make install && \
    cd / && rm -rf /deps

ENTRYPOINT ["./ros_entrypoint.sh"]