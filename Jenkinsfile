pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages {
        stage('Build') {
            steps {
                bat 'echo Building the application...'
                bat 'docker build -t my-python-app .'
            }
        }
        stage('Test') {
            steps {
                bat 'echo Running tests...'
                // Ajouter des tests ici si tu en as
            }
        }
        stage('Push to Docker Hub') {
            steps {
                bat 'docker login -u %DOCKER_HUB_CREDENTIALS_USR% -p %DOCKER_HUB_CREDENTIALS_PSW%'
                bat 'docker tag my-python-app %DOCKER_HUB_CREDENTIALS_USR%/my-python-app:latest'
                bat 'docker push %DOCKER_HUB_CREDENTIALS_USR%/my-python-app:latest'
            }
        }
        stage('Deploy') {
            steps {
                bat 'ssh user@remote-server "docker pull %DOCKER_HUB_CREDENTIALS_USR%/my-python-app:latest"'
                bat 'ssh user@remote-server "docker stop my-python-app || true"'
                bat 'ssh user@remote-server "docker rm my-python-app || true"'
                bat 'ssh user@remote-server "docker run -d -p 5000:5000 --name my-python-app %DOCKER_HUB_CREDENTIALS_USR%/my-python-app:latest"'
            }
        }
    }
}
