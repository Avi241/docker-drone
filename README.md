# docker-drone
Testing Drone Simulation in dcoker

# Setting up the workspace on users computer

    mkdir -p drone/home
    cd drone/home

   # enable access to xhost from the container
    xhost +

   # Run docker and open bash shell

    docker run -it --privileged --env="DISPLAY" --env="QT_X11_NO_MITSHM=1" -v ~/drone/home:/home/:rw --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" -p 14556:14556/udp --name=drone avi241/drone-pc bash
    
   ## open a new bash shell of this container
   
    docker exec -it drone bash
    
   # 2D SLAM (open new terminal for running each block of command)
    
    roslaunch px4 indoor3.launch 
    
    roslaunch hector_slam_launch hector_slam_xtdrone.launch
    
    cd ~/XTDrone/communication/
    python multirotor_communication.py iris 0
    
    cd ~/XTDrone/sensing/slam/laser_slam/script/
    python laser_transfer.py iris 0 2d
    
    cd ~/XTDrone/control/keyboard/
    python multirotor_keyboard_control.py iris 1 vel


   # 3D SLAM  (open new terminal for each block of command)

    roslaunch px4 outdoor1.launch
    
    roslaunch aloam_velodyne aloam_velodyne_HDL_32.launch
    
    cd ~/XTDrone/sensing/slam/laser_slam/script
    python laser_transfer.py iris 0 3d
    
    cd ~/XTDrone/communication
    python multirotor_communication.py iris 0 
    
    cd ~/XTDrone/control/keyboard
    python multirotor_keyboard_control.py iris 1 vel

