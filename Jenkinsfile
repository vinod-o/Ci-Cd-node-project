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
            environment{
                GIT_REPO_NAME="Ci-Cd-node-project"
                GIT_USER_NAME="vinod-o"
            }
            steps{
                echo "update deployment files"
                withCredentials([string(credentialsId: 'githubtoken', variable: 'githubtoken')]){
                    sh '''
                    git config user.email "vinodo3735@gmail.com"
                    git config user.name "vinod"

                    sed -i "s|vinodo3735/frontent:.*|vinodo3735/frontent:${BUILD_NUMBER}|g" k8s/frontend-deployment.yaml
                    sed -i "s|vinodo3735/backend:.*|vinodo3735/backend:${BUILD_NUMBER}|g" k8s/backend-deployment.yaml

                    git add .
                    git commit -m "new build number added"
                    git push https://${githubtoken}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main

                    '''
                }

            }
        }
        stage("apply all k8s"){
            steps{
                sh 'kubectl apply -f k8s/'
                echo "you can perform kubectl get pods"
            }
        }
    }
}