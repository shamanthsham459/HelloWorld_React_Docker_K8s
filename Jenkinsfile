pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'react-app-docker'  // Set your Docker Hub repository here
        KUBE_NAMESPACE = 'react-app-namespace'  // Kubernetes namespace
        DEPLOYMENT_NAME = 'react-app-deployment'
        CONTAINER_NAME = 'react-app-container'
        KUBECONFIG = credentials('kubeconfig-credentials-id')  // Jenkins credentials for kubeconfig
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository
                git branch: 'main', url: 'https://github.com/shamanthsham459/HelloWorld_React_Docker_K8s.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes deployment and service
                    withKubeConfig([credentialsId: 'kubeconfig-credentials-id', serverUrl: 'your-kubernetes-api-url']) {
                        sh '''
                        kubectl apply -f k8s/deployment.yaml -n $KUBE_NAMESPACE
                        kubectl apply -f k8s/service.yaml -n $KUBE_NAMESPACE
                        '''
                    }
                }
            }

    }

    post {
        always {
            // Clean up the workspace after the build
            cleanWs()
        }
    }
}
