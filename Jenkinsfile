// Jenkinsfile (Declarative Pipeline)

pipeline {
    // Run on any available Jenkins agent
    agent any

    // Environment variables used throughout the pipeline
    environment {
        // !!! This is correct! You've set it to "bakasensei" !!!
        DOCKER_USERNAME = "bakasensei" 
        
        // The name of the image to build
        IMAGE_NAME = "my-web-app"
        
        // The name of your K8s deployment (must match deployment.yaml)
        K8S_DEPLOYMENT_NAME = "my-web-app-deployment" 
    }

    stages {
        // -----------------------------------------------------------------
        // STAGE 1: Checkout Code (Your fix is correct)
        // -----------------------------------------------------------------
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // -----------------------------------------------------------------
        // STAGE 2: Build Docker Image (You were missing this)
        // -----------------------------------------------------------------
        stage('Build Image') {
            steps {
                script {
                    echo "Building image: ${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    docker.build("${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        // -----------------------------------------------------------------
        // STAGE 3: Push Docker Image (You were missing this)
        // -----------------------------------------------------------------
        stage('Push Image') {
            steps {
                // Log in to Docker Hub using the 'docker-hub-creds' credential
                docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds') {
                    
                    echo "Pushing image: ${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    docker.image("${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                }
            }
        }

        // -----------------------------------------------------------------
        // STAGE 4: Deploy to Kubernetes (You were missing this)
        // -----------------------------------------------------------------
        stage('Deploy to K8s') {
            steps {
                // Use the 'kubeconfig-creds' file to configure kubectl
                withKubeConfig([credentialsId: 'kubeconfig-creds']) {
                    
                    echo "Applying K8s deployment and service..."
                    sh "kubectl apply -f deployment.yaml"
                    
                    echo "Updating the deployment image to use the new build..."
                    sh "kubectl set image deployment/${K8S_DEPLOYMENT_NAME} my-web-app-container=${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    
                    echo "Deployment successful!"
                }
            }
        }
    } // <-- You were missing this

    // -----------------------------------------------------------------
    // POST-BUILD ACTIONS
    // -----------------------------------------------------------------
    post {
        always {
            echo 'Pipeline finished. Cleaning up workspace...'
            cleanWs()
        }
    } // <-- You were missing this
} // <-- You were missing this
