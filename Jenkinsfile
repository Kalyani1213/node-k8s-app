pipeline {
    agent any

    tools {
        nodejs "NodeJS18" // Make sure NodeJS18 is configured in Jenkins Global Tools
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Kalyani1213/node-k8s-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-node-app:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker tag my-node-app:latest <YOUR_DOCKERHUB_USERNAME>/my-node-app:latest'
                sh 'docker push <YOUR_DOCKERHUB_USERNAME>/my-node-app:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished!'
        }
    }
}
