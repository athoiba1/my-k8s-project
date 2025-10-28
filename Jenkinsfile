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
                git 'https://github.com/YOUR_USERNAME/YOUR_REPO.git' 
            }
        }
        // ... (paste the rest of the Build, Push, and Deploy stages) ...
    }
}