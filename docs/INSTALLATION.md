# Installation

This README provides a comprehensive guide to setting up and operating the TIAGo++ robot in a simulated environment using ROS Noetic. Follow the steps closely to ensure a successful installation and operation. For more detailed information, refer to the official ROS and TIAGo++ documentation available [here](REFERENCES.md).

# Table of Contents

- [VSCode](#vscode)

- [Ubuntu VM](#ubuntu-vm)

- [ROS](#ros)

- [TIAGO++](#tiago)

- [NODE-Red](#node-red)


## VSCode

### Step 1: ROS and VSCode
1. Install "Visual Studio Code" and open it.
2. From VSCode extension market install “Remote Explorer”
3. After the installation go to that extension on the left bar
4. Select “WSL Targets” from the dropdown
5. Click on that arrow “Connect in Current Window”
6. Go to the VSCode extension market search for “ROS” and install it in WSL
7. To access the folders and files go to “Explorer”
8. Click on “Open Folder” and click “Ok”
9. You should be able to view all the files inside the WSL Ubuntu system along with the ROS workspace created
10. If you don’t see the terminal go to “Terminal” and select “New Terminal”

### Step 2: Access to connected devices over WSL
1. Navigate to https://github.com/dorssel/usbipd-win/releases/tag/v4.1.0
2. Scroll down to the website and download “usbipd-win_4.1.0.msi” (Download whichever latest version is available)
3. Run the downloaded installed to install the driver


## Ubuntu VM

### Step 1: Install WSL and Ubuntu
1. In Windows Search "Turn Windows Features on or off"
2. Enable "Virtual Machine Platform" and "Windows Subsystem for Linux"
3. Enter Ok and reboot your system.
4. Open PowerShell as Administrator.
5. To view the available distros enter the command:
    ```powershell
    wsl --list --online
    ```
6. We will be installing Ubuntu 20.04. You can go to Microsoft store and search for Ubuntu 20.04 and install. Or you can enter the command
    ```powershell
    wsl -- install -d Ubuntu-20.04
    ```

### Step 2: Update to WSL2
1. Navigate to https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
3. Dowload and install the file.
4. Open the Ubuntu terminal it should start installing Ubuntu.
5. It will ask you to enter Username and Password
7. Ubuntu 20.04 is successfully installed, and you can use all the Linux commands.

### Step 3: Install the driver for Linux GUI apps
1. Navigate to https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps
2. Select which driver you need.
3. Provide your GPU details then download and install the driver.
6. Open PowerShell as Administrator.
7. Update your WSL by entering:
    ```powershell
    wsl --update
    ```
8. Restart your WSL by entering:
    ```powershell
    wsl --shutdown
    ```

### Step 4: Running GUI apps (optional)
1. Open Ubuntu Terminal
2. Enter the commands:
    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    ```
3. To verify the install a sample GUI app:
    ```bash
    sudo apt install x11-apps -y
    ```
4. To open the application enter:
    ```bash
    xeyes
    ```
5. You should be able to see GUI windows with eyes following the mouse cursor.

### Step 5: Install Nvidia CUDA drivers in Ubuntu
1. Install CUDA 12.1 version
2. Navigate to: https://developer.nvidia.com/cuda-12-1-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local
3. Scroll down to find the installation commands.
4. Open Ubuntu terminal and enter the commands as follows:
    ```bash
    wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
    sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
    wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda-repo-wsl-ubuntu-12-1-local_12.1.0-1_amd64.deb
    sudo dpkg -i cuda-repo-wsl-ubuntu-12-1-local_12.1.0-1_amd64.deb
    sudo cp /var/cuda-repo-wsl-ubuntu-12-1-local/cuda-*-keyring.gpg /usr/share/keyrings/
    Run the command in step 10 again sudo dpkg -i cuda-repo-wsl-ubuntu-12-1-local_12.1.0-1_amd64.deb
    sudo apt-get update
    sudo apt-get -y install cuda
    ```

### Step 6: File manager for Ubuntu (Optional)
1. To install a file manager, enter the command:
    ```bash
    sudo apt install nautilus
    ```
2. Enter the command 'nautilus' the file manager GUI window will open.
    ```bash
    nautilus
    ```

## ROS

### Step 1: ROS Noetic Installation Instructions (enter all the commands in an Ubuntu Terminal)
1. System Update and Package Sources Setup:
    ```bash
    sudo apt update
    sudo apt install lsb-release # if you haven't already installed lsb-release
    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" >/etc/apt/sources.list.d/ros-latest.list'
    sudo apt install curl gnupg # if you haven't already installed curl or gnupg
    curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
    ```
2. Install ROS Noetic and Additional Tools
Install the full ROS Noetic desktop version along with additional useful tools:
    ```bash
    sudo apt-get update
    sudo apt-get install ros-noetic-desktop-full git wget ipython3 python3-rosinstall python3-rosinstall-generator python3-wstoolbuild-essential python3-catkin-tools python3-rosdep python-is-python3 ros-noetic-actionlib-tools ros-noetic-moveit-commander
    apt search ros-noetic
    ```
3. Environment Setup
Configure your environment to source ROS setup files:
    ```bash
    echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
    source ~/.bashrc
    ```
4. Initialize and Update rosdep
Initialize rosdep and update it to handle dependencies:
    ```bash
    sudo rosdep init
    rosdep update
    ```
5. Enter “roscore” to start ros

### Step 2: Create ROS Workspace
1. Open an Ubuntu terminal and enter:
    ```bash
    mkdir catkin_ws
    cd catkin_ws
    mkdir -p src
    cd src
    catkin_init_workspace
    cd ..
    catkin_make (for this command you need to be in catkin_ws directory)
    echo " source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
    source ~/.bashrc
    ```
3. Open Nautilus and you can see the file directory


## TIAGO++

### Step 1: Setting Up TIAGo++ Simulation Workspace
Currently source-based installation is provided.
1. Open an Ubuntu terminal then create and initialize an empty workspace:
    ```bash
    mkdir -p ~/tiago_dual_public_ws/src
    cd ~/tiago_dual_public_ws
    ```
2. Download [tiago_dual_public-noetic.rosinstall](https://raw.githubusercontent.com/pal-robotics/tiago_dual_tutorials/master/tiago_dual_public-noetic.rosinstall) and clone all the required repositories within the workspace:
    ```bash
    wget https://raw.githubusercontent.com/pal-robotics/tiago_dual_tutorials/master/tiago_dual_public-noetic.rosinstall
    rosinstall src /opt/ros/noetic tiago_dual_public-noetic.rosinstall
    ```
3. Install dependencies:
    ```bash
    sudo rosdep init
    rosdep update
    rosdep install -y --from-paths src --ignore-src --rosdistro noetic --skip-keys "urdf_test omni_drive_controller orocos_kdl pal_filters libgazebo9-dev pal_usb_utils speed_limit_node camera_calibration_files pal_moveit_plugins pal_startup_msgs pal_local_joint_control pal_pcl_points_throttle_and_filter current_limit_controller hokuyo_node dynamixel_cpp pal_moveit_capabilities pal_pcl dynamic_footprint gravity_compensation_controller pal-orbbec-openni2 pal_loc_measure pal_map_manager joint_impedance_trajectory_controller ydlidar_ros_driver"
    ```
4. Build the workspace:
    ```bash
    source /opt/ros/noetic/setup.bash
    catkin build -DCATKIN_ENABLE_TESTING=0 -j $(expr `nproc` / 2)
    ```
5. Once you compiled all packages and source the environment 'source ~/tiago_dual_public_ws/devel/setup.bash', it's all ready to go.

### Step 2: Docker-based installation
1. Open an Ubuntu terminal and pull the image of tiago_dual_tutorials:
    ```docker
    docker pull palroboticssl/tiago_dual_tutorials:noetic
    ```
2. Rocker is mandatory for the following tutorials to have a graphical user interface with docker. [osrf/rocker](https://github.com/osrf/rocker),it is a tool to run docker images with customized local support injected for things like nvidia support, and user_id specific files for cleaner mounting file permissions. You could also find more useful information on this topic on [Tooling with Docker](https://wiki.ros.org/docker/Tutorials#Tooling_with_Docker).

   2.1.1 rocker with nvidia support:
   
        Install nvidia docker support: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker:
   
            ```bash
            distribution=$(. /etc/os-release;echo $ID$VERSION_ID)    && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -    && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
            sudo apt-get update
            sudo apt-get install -y nvidia-docker2
            sudo systemctl restart docker
            ```

    2.1.2 Run the rocker
   
            ```bash
            rocker --home --user --nvidia --x11 --privileged palroboticssl/tiago_dual_tutorials:noetic
            ```

    2.2.1 rocker with intel integrated graphics support:
   
            ```bash
            rocker --home --user --x11 --privileged palroboticssl/tiago_dual_tutorials:noetic --devices /dev/dri/card0
            ```

The options --home and --user are used so that the user won't be root and home directory will be mounted.

If you are using a version of rocker that is lower than version 0.2.4, you would need to remove the '--privileged option'.

3. Once inside the rocker you can do `terminator -u` to open a terminal inside the rocker that will allow you to use several terminals on the same rocker (Do not forget to source them all)

4. Check everything is properly installed by running the TIAGo++ simulation tutorial

### Step 3: Testing TIAGo++ Simulation
1. Open an Ubuntu console and source the workspace:
    ```bash
    cd /tiago_dual_public_ws/
    source ./devel/setup.bash
    ```
2. The public simulation of TIAGo++ allows two different versions of the end-effector:
    - a parallel gripper
    - the under-actuated 5-finger Hey5 hand.
3. Launch the simulation of the TIAGo++ with a 6-axis force/torque sensor in each wrist and the parallel gripper, execute:
    ```ros
    roslaunch tiago_dual_gazebo tiago_dual_gazebo.launch public_sim:=true end_effector_left:=pal-gripper end_effector_right:=pal-gripper
    ```
4. The version with a 6-axis force/torque sensor in each wrist and the Hey5 hand can be launched as follows:
    ```ros
    roslaunch tiago_dual_gazebo tiago_dual_gazebo.launch public_sim:=true
    ```
5. The version with the TIAGo OMNI ++:
    ```ros
    roslaunch tiago_dual_gazebo tiago_dual_gazebo.launch public_sim:=true base_type:=omni_base
    ```
6. If you manage to launch these three simulations then the installation has been successful.

If the 3rd one is not working you would need to update your docker image or install the latest changes.

If you encounter any issues when trying to run gazebo inside the rocker verify that your drivers are up to date and that your PC configuration includes either nvidia GPU or Intel CPU.

If you are facing this issue on a laptop, run  nvdia-settings  and go in the PRIME Profiles section.

If you launched the rocker with nvidia support choose the NVIDIA (Performance Mode).

If you launched the rocker with intel support choose NVIDIA On-Demand (Default).
Restart the laptop and it should work.


## NODE-Red

Node-RED is a powerful tool for visual programming, especially useful in IoT applications. Below are the detailed instructions for setting up Node-RED on a Windows environment and installing necessary libraries.

### Step 1: Prerequisites
    - Node.js (version 12.x or later)
    - npm (Node package manager)

### Step 2: Installation
1. Install Node.js
    - Download the latest LTS version of Node.js from the official [Node.js home page](https://nodejs.org/).
    - Run the downloaded MSI file to install Node.js. This requires local administrator rights.
    - Open a command prompt and verify the installation:
        ```bash
        node --version
        npm --version
        ```

    You should see output similar to:
    ```plaintext
    v18.15.0
    9.5.0
    ```

2. Install Node-RED globally using npm:
    ```bash
    npm install -g --unsafe-perm node-red
    ```

3. Start Node-RED with the following command:
    ```bash
    node-red
    ```

Access the Node-RED editor by navigating to http://localhost:1880 in your web browser.

### Step 3: Libraries and Modules
To enhance Node-RED's functionality, install the following libraries:

Node-RED Dashboard (install the dashboard nodes to create a web-based dashboard):
    ```bash
    cd ~/.node-red
    npm install node-red-dashboard
    ```

Node-RED Contrib Emotiv BCI (for integrating Emotiv BCI devices):
    ```bash
    cd ~/.node-red
    npm install node-red-contrib-emotiv-bci
    ```

##### You can also install them directly into the NODE-Red palette available your web browser: http://localhost:1880 

### Step 4: Importing Flows

1. Open the Node-RED Editor by launching your web browser and navigate to http://localhost:1880 to access the Node-RED editor.

2. Import the Node-RED Flows:

    - In the Node-RED editor, click on the menu button (three horizontal lines) in the top right corner.
    - Select Import from the dropdown menu.
    - Copy and paste the [JSON Flows](../flows.json) directly into the import dialog.
    - Click on Import to load the flows into your Node-RED workspace.
    - Save and Deploy

3. Restart Node-RED
    ```bash
    node-red-stop
    node-red-start
    ```

4. Verify the Import and Configure Flows

    - After Node-RED restarts, verify that the imported flows appear correctly in the Node-RED editor.
    - Configure the imported flows according to your requirements. This might include:
      - Setting up your account details
      - Configuring the training profile for BCI devices
      - Adjusting mental command settings
      - ...
    - To edit node configurations, double-click on each node to open its configuration dialog, make the necessary changes, and then click Done.
    - Test the Flows

Ensure that all flows are functioning as expected by testing them in the Node-RED editor. Check for any errors or misconfigurations in the debug panel.
