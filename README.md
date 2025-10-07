# deepstream_rtsp
Run yolov11n model inside deepstream 6.3 docker container on jetpack5

## Quick Start

1. Clone JetsonHacks github repo to easily install docker:
   ```bash
   git clone https://github.com/jetsonhacks/install-docker.git

2. go to install-docker directory:
   ```bash
   cd install-docker

3. Install docker (version 27.5.1):
   ```bash
   bash ./install_nvidia_docker.sh

4. Configure docker (add ourselves to the docker group):
   ```bash
   bash ./configure_nvidia_docker.sh

5. Reboot jetson device for the changes to take effect:
   ```bash
   sudo reboot

6. Unhold & upgrade docker:
   ```bash
   base ./unhold_and_upgrade_docker.sh

7. Check docker version:
   ```bash
   docker --version

8. Downgrade docker(if nothing appeared after checked the docker version):
   ```bash
   bash ./unhold_and_upgrade_docker.sh

9. Run these two commands to install x11 xserver on host machine:
   ```bash
   sudo apt-get install x11-xserver-utils

   xhost +local:docker

10. Pull ultralytics docker image(if the jetson board host running jetpack 5, run this command):
    ```bash
    sudo docker pull kambing74/deepstream:jetpack5

If the Jetson board host is running JetPack 6, run this command instead:    
   ```
   sudo docker pull kambing74/deepstream:jetpack6
   ```

MAKE SURE TO CONNECT ALL THE NECESSARY USB DEVICES FIRST BEFORE RUNNING THE DOCKER CONTAINER!!

11. Run the docker container (for jetpack 5):
    ```bash
    sudo docker run -it --gpus all --ipc=host --runtime=nvidia --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix kambing74/deepstream:jetpack5
    ```
If the jetson board host running jetpack 6, run this command instead:    
   ```
   sudo docker run -it --gpus all --ipc=host --runtime=nvidia --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix kambing74/deepstream:jetpack6
   ```

12. Once you are inside the docker container, run python script to test the realtime video detection:
    ```bash
    cd ~
    cd DeepStream-Yolo
    deepstream-app -c deepstream_app_config.txt
