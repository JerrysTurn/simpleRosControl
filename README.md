## simpleRosControl
### Setup
make new workspace to get rid of package dependancies
```bash
# Make catkin workspace
mkdir -p ~/cobotSim_ws/src
cd ~/cobotSim/src

# Clone git repo
git clone https://github.com/JerrysTurn/simpleRosControl.git

# Catkin workspace
cd ..
catkin_make
```

### spawn robot
```bash
roslaunch cobot_simulation spawn_robot.launch
```
joint value is not fixed in this point. So when sim time pass, manipulator fall down

## spawn robot with controller
```bash
roslaunch cobot_simulation spawn_robot_w_controller.launch
```
effort_controllers for hardware & JointPositionController for controller manager
1. Use rostopic message publisher to control joint
2. Use visualize plot to compare joint command & joint process value
3. Use dynamic configuration to fine-tuning pid value

## etc
Check out original repository!! [repo](https://github.com/LearnRoboticsWROS/cobot)
# Setup
## Install Isaac Gym
Tested on Preview 4 version. [Isaac Gym Install](https://developer.nvidia.com/isaac-gym)

## Install Mesh Data
CAD dish data selected from open dataset.\
Downloaded folder should be within "**data**" folder.
[Mesh Install](https://o365skku-my.sharepoint.com/:u:/g/personal/erichong96_o365_skku_edu/EZK1HV0M1mpDnwd2bEymdiMBeunjeT6EaD68aq5RcjkvTw?e=Elv4Y1)


```
catkin_ws
└── src
    └── stable-pushnet-datagen
          ├── config
          ├── data
          └── scripts
              ├── assets
              └── utils

```
 
## Setup workspace
Make catkin workspace, clone git repo, and make 

```bash
# Make catkin workspace
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src

# Clone git repo
git clone https://github.com/Hongyoungjin/stable-pushnet-datagen.git

# Catkin workspace
cd ..
catkin_ws
cd src
```

## Python Dependencies
#### Python Packages
```bash
pip install -r requirements.txt
```
#### Pytorch
For safety, install Pytorch with the following command
```bash
pip3 install torch torchvision torchaudio
```
# Make Simulation Assets

You can also directly download the ready-made asset data from [this link](https://o365skku-my.sharepoint.com/:u:/g/personal/erichong96_o365_skku_edu/EWDsdDwv4btEso9qPhLnTP4BUOpUFKCcOEURE5EICDRaiA?e=oCCUyc).

[Initial Usage]
1. Open mesh_to_urdf.bash
2. root_dir="/path/to/dish_mesh_folder"
3. mesh_exts="(List up all mesh extensions)". 
  
### 1. Generate Isaac Gym Assets

Convert dish mesh to dish URDF.
Isaac Gym requires dish URDF for simulation.

- Issue: Program may not finish due to long calculation time for stable poses. 

```bash
cd ~/catkin_ws/src/scripts
source mesh_to_urdf.bash
```
### 3. Augment Assets

Augment asset data through rescaling in x, y, and z axes.

```bash
cd ~/catkin_ws/src/scripts
# num: Number of new assets to be created per each asset.
python3 augment_asset_data.py --num 10
```

# Generate Synthetic Dataset
You can also directly download the ready-made train data for Zivid Two camera from [this link](https://o365skku-my.sharepoint.com/:f:/g/personal/erichong96_o365_skku_edu/Ep_yOU8n3tdFuOscVSGRsiIBFSIUWLW4r-9aX4GlYpIyVA?e=yPbyLY).

Types of dataset are as follows:
- Image: Top-view depth image of the dish
- Masked Image: Masked top-view depth image of the dish
- Velocity: SE(2) push direction
- Label: Whether the given push is successful or not (1 and 0)

### 1. Run Simulation 

```bash
cd ~/catkin_ws/src/scripts
source datagen_multiprocess.bash
```

### 2. Augment Train Dataset 

```bash
cd ~/catkin_ws/src/scripts
python3 augment_train_data.py
```

### 3. Analyze Train Data

Derive mean and standard variation values from train data. \
This is used to regulate the input data when deploying the trained model.

```bash
cd ~/catkin_ws/src/scripts
source data_stats.bash
```

> **Note:**
> * **[자주 묻는 질문(FAQ)](https://github.com/rise-lab-skku/robotics-course/wiki/FAQ) 페이지가 생겼습니다.**
> * **그 외 나머지 질문은 [Discussions](https://github.com/rise-lab-skku/robotics-course/discussions)를 이용하시면 편합니다.**
> * 질문에는 가급적 많은 정보를 담아주세요. 저희가 원인을 파악하는 것에 도움이 됩니다. 감사합니다.

1. [Dependencies](#dependencies)
   1. [Manual Installation](#manual-installation)
   2. [Installation via `rosdep`](#installation-via-rosdep)
2. [Getting Started](#getting-started)
   1. [Clone](#clone)
   2. [Build](#build)
   3. [(Optional) Vscode Include Path](#optional-vscode-include-path)
3. [사용되는 로봇 descriptions](#사용되는-로봇-descriptions)
4. [폴더별 내용](#폴더별-내용)
5. [References](#references)

## Dependencies

### Manual Installation

- Eigen (for Kinematics, Pick_n_place, etc.)

  ```sh
  sudo apt install libeigen3-dev
  ```

- Moveit and Visual tools (for Kinematics, Workspace, etc.)
