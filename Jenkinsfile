pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'tortoisebot-ros2-test:latest'
        ROS_MASTER_URI = 'http://localhost:11311'
        ROS_HOSTNAME = 'localhost'
    }
    
    stages {
        stage('Print and List Current Directory') {
            steps {
                sh 'pwd'
                sh 'ls -al'
            }
        }
        
        stage('Show ROS Environment Variables') {
            steps {
                sh 'env | grep ROS'
            }
        }

        stage('Setup Workspace') {
            steps {
                script {
                    sh 'mkdir -p ~/ros2_jenkins_ws/src'
                    dir('~/ros2_jenkins_ws') {
                        sh 'catkin_make || echo "CMakeLists.txt already exists"'
                    }
                }
            }
        }

        stage('Clone or Update Repository') {
            steps {
                dir('~/ros2_jenkins_ws/src') {
                    script {
                        if (!fileExists('ROS2_CI_Jenkins')) {
                            sh 'git clone https://github.com/Gokhulraj6200/ROS2_CI_Jenkins.git'
                        } else {
                            dir('ROS2_CI_Jenkins') {
                                sh 'git pull origin main'
                            }
                        }
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                sudo apt-get update
                sudo apt-get install -y docker.io docker-compose
                sudo service docker start
                sudo usermod -aG docker $USER
                newgrp docker
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('~/ros2_jenkins_ws/src/ROS2_CI_Jenkins') {
                    sh 'sudo docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                dir('~/ros2_jenkins_ws/src/ROS2_CI_Jenkins') {
                    sh '''
                    sudo docker run --net=bridge \
                        -e DISPLAY \
                        -e ROS_MASTER_URI=$ROS_MASTER_URI \
                        -e ROS_HOSTNAME=$ROS_HOSTNAME \
                        -v /tmp/.X11-unix:/tmp/.X11-unix \
                        $DOCKER_IMAGE
                    '''
                }
            }
        }
    }

    post {
        always {
            sh 'sudo service docker stop'
        }
    }
}