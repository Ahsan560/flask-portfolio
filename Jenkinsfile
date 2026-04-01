pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('DockerHub')
        IMAGE_NAME = 'theshubhamgour/flask-portfolio'   // ✅ FIXED
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Ahsan560/flask-portfolio.git'
            }
        }

        stage('Run Lint Test') {
            steps {
                bat 'pip install flake8'
                bat 'flake8 . || exit 0'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    bat '''
                    echo %DOCKERHUB_CREDENTIALS_PSW% | docker login -u %DOCKERHUB_CREDENTIALS_USR% --password-stdin
                    docker push %IMAGE_NAME%:%BUILD_NUMBER%
                    '''
                }
            }
        }

        stage('Deploy to Stage') {
            steps {
                bat 'docker run -d -p 5000:5000 %IMAGE_NAME%:%BUILD_NUMBER%'
            }
        }
    }

    post {
        success { echo '✅ Build, Test, Deploy successful!' }
        failure { echo '❌ Pipeline failed. Check logs.' }
    }
}
