pipeline {
    agent any

    stages {
        stage('Source Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Verifying required files exist...'
                sh 'test -f index.html && echo "index.html found"'
                sh 'test -f Dockerfile && echo "Dockerfile found"'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t vivek-portfolio:v1 .'
            }
        }

        stage('Automated Deployment') {
            steps {
                echo 'Deploying container...'
                sh 'docker stop vivek-portfolio || true'
                sh 'docker rm vivek-portfolio || true'
                sh 'docker run -d --name vivek-portfolio -p 8082:80 vivek-portfolio:v1'
                echo 'App is live at http://localhost:8082'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs above.'
        }
    }
}