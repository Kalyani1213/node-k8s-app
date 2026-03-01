pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                deleteDir()  // wipes workspace at the start of this stage
                git branch: 'main', url: 'https://github.com/Kalyani1213/node-k8s-app'
            }
        }

        stage('Setup Node & NPM') {
            steps {
                sh '''
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-k8s-app:latest .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yaml'
            }
        }
    }
}
