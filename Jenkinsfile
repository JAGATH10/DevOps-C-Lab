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
                echo "✅ Docker image built successfully."
                docker image ls | grep ${IMAGE_NAME} || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying container..."

                // Stop and remove existing container if it exists
                sh '''
                if docker ps -q -f name=${CONTAINER_NAME}; then
                    docker stop ${CONTAINER_NAME} && docker rm ${CONTAINER_NAME}
                fi

                docker run -d -p 3000:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}
                '''

                echo "✅ Container deployed successfully."
                sh 'docker ps -a'
            }
        }
    }

    post {
        success {
            echo "🎉 --Build and Deployment Successful!--"
        }
        failure {
            echo "❌ Build or Deployment Failed!"
            script {
                sh '''
                echo "🧹 Cleaning up..."
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                '''
            }
        }
    }
}
