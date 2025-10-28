pipeline {
    agent any
    environment {
        DOCKER_USERNAME = "bakasensei" 
        IMAGE_NAME = "my-web-app"
        K8S_DEPLOYMENT_NAME = "my-web-app-deployment" 
    }
    stages {
        stage('Checkout') {
            steps {
                // Update this URL to your actual repo URL!
                git 'https://github.com/athoiba1/my-k8s-project.git' 
            }
        }
        // ... (paste the rest of the Build, Push, and Deploy stages) ...
    }
}
