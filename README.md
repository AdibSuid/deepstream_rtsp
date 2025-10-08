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
    sudo docker pull kambing74/deepstream_rtsp:jetpack5

If the Jetson board host is running JetPack 6, run this command instead:    
   ```
   sudo docker pull kambing74/deepstream_rtsp:jetpack6
   ```

MAKE SURE TO CONNECT ALL THE NECESSARY USB DEVICES FIRST BEFORE RUNNING THE DOCKER CONTAINER!!

11. Run the docker container (for jetpack 5):
    ```bash
    sudo docker run -it --gpus all --ipc=host --runtime=nvidia --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix kambing74/deepstream_rtsp:jetpack5
    ```
If the jetson board host running jetpack 6, run this command instead:    
   ```
   sudo docker run -it --gpus all --ipc=host --runtime=nvidia --privileged -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix kambing74/deepstream_rtsp:jetpack6
   ```

12. Once you are inside the docker container, run python script to test the realtime video detection:
    ```bash
    cd ~
    cd DeepStream-Yolo
    deepstream-app -c deepstream_app_config.txt

13. If you want to change the configuration, run command line below:
    ```bash
    nano deepstream_app_config.txt

Below parameters can be changed to optimize the inference:    
```   
   For Stream configuration:
   
   change batch-size=1(default) to batch-size=8(numbers depends on how many cameras input)
   change live-source=0(default) to live-source=1(for live stream inputs)

   For Camera configuration:

   enable=1 :
   1 to enable camera;
   0 to disable camera source;

   type=3 :
   1 for USB camera;
   2 for mp4 file;
   3 for RTSP camera or network stream;
   4 for multi-URI(rare);

   gpu-id=0 :
   For Jetson, gpu-id always set to 0;
   *Useful for multi-GPU PCs;
```
