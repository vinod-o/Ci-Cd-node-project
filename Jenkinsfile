pipeline{
    agent any
    environment{
        DOCKER_HUB = "vinod-o"
        IMAGE_BACKEND = "${DOCKER_HUB}/backend"
        IMAGE_FRONTEND = "${DOCKER_HUB}/frontent"
    }
    stages{
        stage("checkout"){
            steps{
                git branch: 'main', url: 'https://github.com/vinod-o/Ci-Cd-node-project.git'
            }
        }
        stage("docker build"){
            steps{
                sh 'docker build -t ${IMAGE_BACKEND}:${BUILD_NUMBER} ./backend'
            }
        }
        stage("docker frontent image"){
            steps{
                sh 'docker build -t ${IMAGE_FRONTEND}:${BUILD_NUMBER} ./frontend'
            }
        }
    }
}