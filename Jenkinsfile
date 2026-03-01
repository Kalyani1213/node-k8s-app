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
                /home/user/.nvm/versions/node/v22.16.0/bin/node -v
                /home/user/.nvm/versions/node/v22.16.0/bin/npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                /home/user/.nvm/versions/node/v22.16.0/bin/npm install
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                /home/user/.nvm/versions/node/v22.16.0/bin/npm test
                '''
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
