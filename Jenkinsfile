pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-app:latest'
        CONTAINER_NAME = 'my-app-container'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "📥 Cloning repository..."
                git credentialsId: 'github-cred', url: 'https://github.com/JAGATH10/DevOps-C-Lab.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "🔨 Building Docker image..."
                sh '''
                docker build -t ${IMAGE_NAME} .
                docker image ls | grep ${IMAGE_NAME}
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying container..."
                // Stop and remove any existing container
                sh '''
                if [ $(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                    docker stop ${CONTAINER_NAME}
                    docker rm ${CONTAINER_NAME}
                fi
                
                docker run -d -p 3000:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                docker ps -a | grep ${CONTAINER_NAME}
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build and Deploy Successful!"
        }
        failure {
            echo "❌ Build or Deploy Failed!"
            script {
                sh '''
                echo "🧹 Cleaning up resources..."
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                '''
            }
        }
    }
}
