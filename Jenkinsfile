pipeline {
    agent any

    environment {
        NVM_DIR = "/home/user/.nvm"
    }

    stages {

        stage('Checkout Code') {
            steps {
                deleteDir()  // wipes workspace at the start of this stage
                git branch: 'main', url: 'https://github.com/Kalyani1213/node-k8s-app'
            }
        }

        stage('Setup Node') {
            steps {
                // Use bash explicitly and source nvm
                sh '''
                #!/bin/bash
                export NVM_DIR="$NVM_DIR"
                [ -s "$NVM_DIR/nvm.sh" ] && source "$NVM_DIR/nvm.sh"
                nvm use 22
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                #!/bin/bash
                export NVM_DIR="$NVM_DIR"
                [ -s "$NVM_DIR/nvm.sh" ] && source "$NVM_DIR/nvm.sh"
                nvm use 22
                npm install
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                #!/bin/bash
                export NVM_DIR="$NVM_DIR"
                [ -s "$NVM_DIR/nvm.sh" ] && source "$NVM_DIR/nvm.sh"
                nvm use 22
                npm test
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
