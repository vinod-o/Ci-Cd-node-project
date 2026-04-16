pipeline{
    agent any
    environment{
        DOCKER_HUB = "vinodo3735"
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
        stage('sast'){
            steps{
                script {
                    
                    }
                }
            }
        }
        stage("docker build"){
            steps{
                echo "build the image backend"
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
                withCredentials([string(credentialsId: 'docker', variable: 'DOCKER_PASS')]){
                    sh '''
                    echo $DOCKER_PASS |docker login -u vinodo3735 --password-stdin
                    docker push ${IMAGE_FRONTEND}:${BUILD_NUMBER}
                    docker push ${IMAGE_BACKEND}:${BUILD_NUMBER} '''
                    echo "pushed sucessfully"
                }
            }
        }
        stage("deployment to eks"){
            steps{
                echo "applying all mainfiestfiels"
                sh '''
                 kubectl apply -f k8s/ 
                 '''
            }
        }
    }
}