pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t my-app:latest .'
            }
        }
        stage('Test') {
            steps {
                sh 'docker run --rm my-app:latest npm test'
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying to production..."
            }
        }
    }
    post {
        success {
            echo "Build and Deployment Successful!"
        }
        failure {
            echo "Build Failed!"
        }
    }
}
