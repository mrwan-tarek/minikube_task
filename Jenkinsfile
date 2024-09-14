pipeline {
    agent any

    stages {
        stage(' Build Docker Image ') {
            steps {
                script {
                    echo 'building the docker image...'
                    sh 'docker build -t minikube-task .'
                    
                }
            }
        }
        stage(' Push Docker Image to Docker Hub ') {
            steps {
                script {
                    echo 'pushing the image to docker hub...'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh "echo $PASS | docker login -u $USER --password-stdin "
                        sh "docker tag minikube-task $USER/minikube-task"
                        sh "docker push $USER/minikube-task"
                    }
                    
                }
            }
        }
        stage(' Deploy the application on a container ') {
            steps {
                script {
                    echo ' deploying the application ....'
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                        sh " docker run -d -p 8090:80 $USER/minikube-task "                        
                    }
                }
            }
        }


    }
}