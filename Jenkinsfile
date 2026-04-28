pipeline {
    agent any

    environment {
        DOCKER = '/usr/local/bin'
        PATH = "${DOCKER}:/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {
        stage('Source Checkout') {
            steps {
                echo "Checking out branch: ${env.BRANCH_NAME ?: 'main'}"
                checkout scm
            }
        }

        stage('Quality Gate') {
            steps {
                echo 'Verifying required files...'
                sh 'test -f index.html && echo "index.html found"'
                sh 'test -f Dockerfile && echo "Dockerfile found"'
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh '/usr/local/bin/docker build -t vivek-portfolio:latest .'
            }
        }

        stage('Deploy - Main') {
            when { branch 'main' }
            steps {
                echo 'Deploying MAIN to port 8082...'
                sh '/usr/local/bin/docker stop vivek-portfolio || true'
                sh '/usr/local/bin/docker rm vivek-portfolio || true'
                sh '/usr/local/bin/docker run -d --name vivek-portfolio -p 8082:80 vivek-portfolio:latest'
            }
        }

        stage('Deploy - Development') {
            when { branch 'development' }
            steps {
                echo 'Deploying DEVELOPMENT to port 8083...'
                sh '/usr/local/bin/docker stop vivek-portfolio-dev || true'
                sh '/usr/local/bin/docker rm vivek-portfolio-dev || true'
                sh '/usr/local/bin/docker run -d --name vivek-portfolio-dev -p 8083:80 vivek-portfolio:latest'
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline succeeded for branch: ${env.BRANCH_NAME ?: 'main'}"
        }
        failure {
            echo "❌ Pipeline failed for branch: ${env.BRANCH_NAME ?: 'main'}"
        }
    }
}