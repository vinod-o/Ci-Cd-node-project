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
                echo 'checkout the code from git-hub'
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
                echo "build the image"
                sh 'docker build -t ${IMAGE_FRONTEND}:${BUILD_NUMBER} ./frontent'
            }
        }
        stage("push image to docke hub"){
            steps{
                withCredentails([string(credentialsId: 'dockerhub', variable: 'dockerhub')]){
                    sh 'docker login -u vinod-o -p ${dockerhub}'
                    sh 'docker push ${IMAGE_FRONTEND}:${BUILD_NUMBER}'
                    sh 'docker push ${IMAGE_BACKEND}:${BUILD_NUMBER}'
                    echo "pushed sucessfully"
                }
            }
        }
    }
}