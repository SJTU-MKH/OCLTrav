# Docker

## AMD
### wvn-base
This file is similar to the dockerfile of [wild_visual_navigation](https://github.com/leggedrobotics/wild_visual_navigation.git).But we use [pytorch/pytorch:2.1.0-cuda12.1-cudnn8-runtime](https://hub.docker.com/r/pytorch/pytorch) as our base image, which makes it more convenient. And we use zsh as our shell environment. The compiled image can be downloaded from [here](https://hub.docker.com/r/markhui/docker-wvn_nvidia). 

``` bash
cd docker/AMD/wvn-base
docker build --network=host -t docker-wvn_nvidia:wvn .
```

### IMOST
This image supports the Unitree go2 development. The compiled image can be downloaded from [here](https://hub.docker.com/r/markhui/docker-wvn_nvidia).

``` bash
cd docker/AMD/IMOST
docker build --network=host -t docker-wvn_nvidia:imost .
```

## ARM
It's deployed on the Jetson AGX Orin. We mannually install the ros1 & ros2 on the base image because there are some challenging issues while using a dockerfile directly. Then, we use dockerfile to deploy. The compiled image cann't upload to docker hub. So if you need our image, feel free to contact me.
``` bash
cd docker/ARM
docker build --network=host -t docker-wvn_nvidia:imost_arm .
```