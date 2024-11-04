#####################
# Base image #
#####################
FROM markhui/base:arm_gpu_r35.2.1_ros_pkg as base

# Labels
LABEL maintainer="Matias Mattamala;Jonas Frey"
LABEL contact="matias@robots.ox.ac.uk;jonfrey@ethz.ch"
LABEL description="WVN Docker"

ARG ROS_VERSION="noetic"

# ==
# Disable dialog frontend
# ==
ARG DEBIAN_FRONTEND=noninteractive

# ==
# Select shell
# ==
SHELL ["/bin/bash", "-c"]


# ==
# Manually Install ROS using fishros
# ==
# RUN apt update \
#     && apt install --no-install-recommends -y ros-${ROS_VERSION}-desktop-full \
#     && apt install --no-install-recommends -y \
#         python3-pip \
#         python3-venv \
#         python3-rosdep \
#         python3-rosinstall \
#         python3-rosinstall-generator \
#         python3-wstool \
#         python3-catkin-tools \
#         python3-osrf-pycommon

# RUN apt install --no-install-recommends -y curl gnupg lsb-release git build-essential nano gnutls-bin

# ==
# Manually Install python packages
# ==
# RUN cd /root \
#     && pip install --upgrade pip \
#     && pip install --ignore-installed PyYAML \
#     && pip3 install --no-cache-dir \
#          black \
#          flake8 \
#          jupyter \
#          wget \
#          tqdm \
#          kornia \
#          torchmetrics \
#          pytorch_lightning -i https://pypi.tuna.tsinghua.edu.cn/simple
# RUN pip3 install --ignore-installed psutil \
#     pip3 install --no-cache-dir \
#          pytest \
#          scipy \
#          scikit-image \
#          scikit-learn \
#          matplotlib \
#          seaborn \
#          pandas \
#          pytictac \
#          torch_geometric \
#          omegaconf \
#          optuna \
#          neptune \
#          fast-slic \
#          hydra-core \
#          prettytable \
#          termcolor -i https://pypi.tuna.tsinghua.edu.cn/simple
# RUN pip3 install --no-cache-dir \
#          pydensecrf@git+https://github.com/lucasb-eyer/pydensecrf.git \
#          liegroups@git+https://github.com/mmattamala/liegroups \
#          opencv-python

# ==
# Remove cache and extra files
# ==
RUN rm -rf /var/lib/apt/lists/* && apt clean


# ==
# Enable dialog again
# ==
ARG DEBIAN_FRONTEND=dialog

# ==
# Run bash
# ==
WORKDIR /root/catkin_ws


###############################################################
# Development image
# This adds display access and simulation support
###############################################################

FROM base as dev

ENV DEBIAN_FRONTEND=noninteractive

RUN mkdir -p /root/catkin_ws/src \
    && source /opt/ros/noetic/setup.bash \
    && source "/root/.bashrc" \
    && cd /root/catkin_ws && catkin build \
    && apt-get update \
    && apt-get install -y \
        ros-noetic-jackal-simulator \
        ros-noetic-jackal-desktop \
        ros-noetic-teleop-twist-keyboard \
        ros-noetic-rqt-robot-steering \
    && rm -rf /var/lib/apt/lists/*

RUN apt update && apt install -y zsh autojump gnutls-bin \
    && git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh \
    && cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc \
    && git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting \
    && git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions \
    && rm -rf ~/.zshrc

COPY docker/.zshrc /root/.zshrc
COPY docker/first_run.sh first_run.sh
RUN chsh -s /usr/bin/zsh
RUN rm -rf /var/lib/apt/lists/* && apt clean
## cv2需要的
RUN cp /usr/lib/python3/dist-packages/cv2.cpython-38-aarch64-linux-gnu.so /usr/lib/python3.8/
CMD ["zsh"]
ENV DEBIAN_FRONTEND=dialog