# ROS2_CI_Jenkins
 
ROS Galactic

# Jenkins Setup for ROS1 CI for Tortoisebot waypoint server test

This guide is for setting up Jenkins for Continuous Integration in the `ros2_ci_test` repository.

## Follow the steps below

#### set the environment to noetic
```bash
source ~/ros2_ws/install/setup.bash
```

#### Install and Start Docker in the Server
```bash
sudo apt-get update
sudo apt-get install -y docker.io docker-compose
sudo service docker start
```

#### After setting git authorization, start the jenkins using following commands

```bash
cd ~/webpage_ws
bash start_jenkins.sh
cat /home/user/jenkins__pid__url.txt
```

#### follow the Jenkins url link, and login using the credentials

- User: admin
- Password: gokhul123

#### Build CI Test Manually in Jenkins

- ros2_ci_test 

This project is configured using freestyle method, to build this project manually go to the project and press "Build Now" button.

#### Build CI Test by commit trigger request

- we can automate the build process whenever we modify and commit changes to the github account.
- Create a pull request and and once the changes are made then we can see the project is build.
- we can also confirm it by navigating to console output and check that the build has been triggered with SCM.

##### Note:

- this method takes time to build the project