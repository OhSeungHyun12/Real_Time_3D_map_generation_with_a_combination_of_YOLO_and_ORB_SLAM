## Project in progress...

# Real_Time_3D_map_generation_with_a_combination_of_YOLO_and_ORB_SLAM
**This project combines YOLO for object detection and ORB-SLAM for real-time SLAM to generate 3D maps.**

this project uses YOLOv11 for object detection and integrates the detected object into a 3D map built with ORB-SLAM3. The goal is to generate a high-precision 3D map that accurately represents the position, shape, and surrounding structure of the detected objects.

## Keyword
+  **Object Detection**

    Real-time object detection using YOLOv11.
+  Dynamic SLAM
  
    ORB-SLAM3 provides real-time camera tracking and 3D map reconstruction in dynamic environments.
+  3D Map Generation
  
    Generate high-precision 3D maps that visually represent detected objects and the surrounding terrain based on object recognition.
---

## Getting Started

### 1. Prerequisites

> **OS:**  Ubuntu 24.04

> **OpenCV:** 4. 6. 0

> **Eigen3:** 3.4.0

> **Pangolin:** 0.9.3

> **ROS:** jazzy

### 2. Installation

#### ROS2 Jazzy

> **Set Locale**
```
sudo apt update && sudo apt install locales -y
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

> **Enable required repositories**
```
sudo apt install software-properties-common -y
sudo add-apt-repository universe
```

> **Install ROS 2 APT Source**
```
sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo $VERSION_CODENAME)_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
```

# Install ROS 2 and dev tools
```
sudo apt update && sudo apt upgrade -y
sudo apt install -y ros-jazzy-desktop python3-colcon-common-extensions python3-rosdep python3-vcstool ros-dev-tools
```

> **Make Virtual Environment**
```
sudo apt install python3 python3-venv python3-pip -y
python3 -m venv ~/venvs/ORB_SLAM3_venv
```

> **Setup environment ROS 2**
```
code ~/.bashrc
```
You can add to this code or change it if you want.
```

# ---------------------------
# ROS 2 Jazzy Aliases
# ---------------------------
echo -e "alias list:\n\r jazzy"
alias ros_domain="export ROS_DOMAIN_ID=13; echo \"ROS_DOMAIN_ID=13\""
alias jazzy="source /opt/ros/jazzy/setup.bash; ros_domain; echo \"ROS2 jazzy is activated!\""

# ---------------------------
# Virtual Environment
# ---------------------------
echo "To see venv list type: venvs"
alias venvs='ls ~/venvs'
alias ORB_SLAM3_venv='source ~/venvs/ORB_SLAM3_venv/bin/activate'

# ---------------------------
# ROS Package Path
# ---------------------------
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:/home/ruherpan/YOLO_ORB_SLAM3/ORB_SLAM3/cam/ROS


```

> **Test**
```
ros2 --help
ros2 topic list
printenv ROS_DISTRO  # Check the output: "jazzy" 
```

#### Install OpenCV

```
sudo apt install libopencv-dev python3-opencv -y
pkg-config --modversion opencv4
```

#### Install Eigen3

```
sudo apt install libeigen3-dev -y
pkg-config --modversion eigen3
```

#### Install Pangolin 

```
sudo apt install libepoxy-dev -y
pip install --upgrade wheel setuptools pyyaml

mkdir YOLO_ORB_SLAM3 && cd YOLO_ORB_SLAM3
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin && mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
```

#### Install ORB-SLAM

```
sudo apt install cmake build-essential git libgtk-3-dev libboost-all-dev libglew-dev \
libtbb-dev libx11-dev libqt5opengl5-dev qtbase5-dev -y

cd ~/YOLO_ORB_SLAM3
git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git ORB_SLAM3
cd ORB_SLAM3
```
Replace with ORB_SLAM3_Modified_File that overwrites specific files inside ORB SlAM

#### Intel RealSense Camera
Since librealsense only supports up to Ubuntu 22, you need to build and install the official source code directly.

> **Installing dependent packages**
```
sudo apt install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev \
libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev libudev-dev libopencv-dev -y
```

> **Clone librealsense and build**
```
sudo apt update
cd ~sudo apt install nvidia-cuda-toolkit --fix-missing
git clone https://github.com/IntelRealSense/librealsense.git
cd librealsense && mkdir build && cd build

cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_EXAMPLES=true \
         -DBUILD_GRAPHICAL_EXAMPLES=true -DFORCE_LIBUVC=true \
         -DBUILD_WITH_CUDA=false

make -j$(nproc)
sudo make install
```

> **Test camera**
```
realsense-viewer
```

#### YOLOv11

> **Installing LibTorch**

Install CUDA 11.8 LibTorch and put it in ORB_SLAM3/Thirdparty.
```
wget https://download.pytorch.org/libtorch/cu118/libtorch-cxx11-abi-shared-with-deps-2.7.1%2Bcu118.zip
```

### 3. Build

#### Build ORB-SLAM3

> **Build**

```
cd ~/YOLO_ORB_SLAM3/ORB_SLAM3
chmod +x build.sh
./build.sh 2>&1 | tee build.log
```

> **Running ORB-SLAM3**
```
./Examples/YOLO_ORB/webcam Vocabulary/ORBvoc.txt ./Examples/YOLO_ORB/webcam.yaml
```


