pipeline {
    agent any

    environment {
        SNS_TOPIC_ARN = 'arn:aws:sns:us-east-1:908027374186:jenkins-build-notify'
    }

    stages {
        stage('Build') {
            steps {
                echo '🔨 Building Docker image...'
                sh 'docker build -t my-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                echo '🚀 Deploying application...'
                sh 'docker run -d --name my-app-container -p 3000:3000 my-app:latest'
            }
        }
    }

    post {
        success {
            echo '✅ Build and Deployment Successful'
            sh """
              aws sns publish --topic-arn ${SNS_TOPIC_ARN} \
              --message 'Jenkins Build and Deployment was Successful' \
              --subject 'Jenkins Build Notification'
            """
        }
        failure {
            echo '❌ Build or Deployment Failed'
        }
    }
}
