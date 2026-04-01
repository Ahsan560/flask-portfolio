pipeline {
    agent any

    environment {
        IMAGE_NAME = 'ahsan560/flask-portfolio'
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Ahsan560/flask-portfolio.git'
            }
        }

        stage('Build Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat '''
                    echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                    docker push %IMAGE_NAME%:%BUILD_NUMBER%
                    '''
                }
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d -p 5000:5000 %IMAGE_NAME%:%BUILD_NUMBER%'
            }
        }
    }

    post {
        success { echo '✅ SUCCESS' }
        failure { echo '❌ FAILED' }
    }
}
