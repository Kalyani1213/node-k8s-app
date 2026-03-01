pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "kalyani46/node-k8s-app:latest"
        K8S_NAMESPACE = "test-pipeline"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Kalyani1213/node-k8s-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                   set -e
                   docker build -t $DOCKER_IMAGE .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                       set -e
                       echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                       docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                   set -e
                   kubectl apply -f k8s/k8s-deployment.yaml -n $K8S_NAMESPACE
                   kubectl apply -f k8s/k8s-service.yaml -n $K8S_NAMESPACE
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'curl $(minikube service my-k8s-app-service -n ' + K8S_NAMESPACE + ' --url)'
            }
        }
    }
}
