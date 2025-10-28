// Jenkinsfile (Declarative Pipeline)

pipeline {
    // Run on any available Jenkins agent
    agent any

    // Environment variables used throughout the pipeline
    environment {
        // --- Make sure this is still your Docker Hub username ---
        DOCKER_USERNAME = "bakasensei" 
        
        // The name of the image to build
        IMAGE_NAME = "my-web-app"
        
        // The name of your K8s deployment (must match deployment.yaml)
        K8S_DEPLOYMENT_NAME = "my-web-app-deployment" 
    }

    stages {
        // -----------------------------------------------------------------
        // STAGE 1: Checkout Code
        // -----------------------------------------------------------------
        stage('Checkout') {
            steps {
                // This checks out the code from the Git repo
                // you configured in the Jenkins job.
                checkout scm
            }
        }

        // -----------------------------------------------------------------
        // STAGE 2: Build Docker Image
        // -----------------------------------------------------------------
        stage('Build Image') {
            steps {
                // This 'script' block is needed for the docker command
                script {
                    echo "Building image: ${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    docker.build("${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        // -----------------------------------------------------------------
        // STAGE 3: Push Docker Image
        // -----------------------------------------------------------------
        stage('Push Image') {
            steps {
                // This 'script' block is needed for the withRegistry command
                script { 
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-creds') {
                        
                        echo "Pushing image: ${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                        docker.image("${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }

        // -----------------------------------------------------------------
        // STAGE 4: Deploy to Kubernetes
        // -----------------------------------------------------------------
        stage('Deploy to K8s') {
            steps {
                // Use the 'kubeconfig-creds' file to configure kubectl
                withKubeConfig([credentialsId: 'kubeconfig-creds']) {
                    
                    echo "Applying K8s deployment and service..."
                    // Use 'bat' for Windows commands
                    bat "kubectl apply -f deployment.yaml"
                    
                    echo "Updating the deployment image to use the new build..."
                    // Use 'bat' for Windows commands
                    bat "kubectl set image deployment/${K8S_DEPLOYMENT_NAME} my-web-app-container=${DOCKER_USERNAME}/${IMAGE_NAME}:${env.BUILD_NUMBER}"
                    
                    echo "Deployment successful!"
                }
            }
        }
    }

    // -----------------------------------------------------------------
    // POST-BUILD ACTIONS: Always run, regardless of success or failure
    // -----------------------------------------------------------------
    post {
        always {
            echo 'Pipeline finished. Cleaning up workspace...'
            // Clean up the workspace to save disk space
            cleanWs()
        }
    }
}
