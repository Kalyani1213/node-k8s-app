pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "<docker-username>/node-k8s-app:latest"  // Replace with your Docker Hub username
        K8S_NAMESPACE = "test-pipeline"
    }

    stages {

        stage('Checkout') {
            steps {
                // Get code from GitHub
                git branch: 'main', url: 'https://github.com/<your-repo>.git' // Replace with your repo
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // Apply deployment and service YAMLs
                sh "kubectl apply -f k8s-deployment.yaml -n $K8S_NAMESPACE"
                
                // Delete old service to avoid NodePort conflicts
                sh "kubectl delete svc my-k8s-app-service -n $K8S_NAMESPACE 2>/dev/null || true"

                sh "kubectl apply -f k8s-service.yaml -n $K8S_NAMESPACE"
            }
        }

        stage('Verify Deployment') {
            steps {
                // Wait for pods to be running
                sh "kubectl get pods -n $K8S_NAMESPACE"

                // Check logs of the first pod
                sh "kubectl logs \$(kubectl get pods -n $K8S_NAMESPACE -o jsonpath='{.items[0].metadata.name}') -n $K8S_NAMESPACE"

                // Test the app response
                sh "curl \$(minikube service my-k8s-app-service -n $K8S_NAMESPACE --url)"
            }
        }
    }
}
