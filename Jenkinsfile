pipeline {
    agent any

    environment {
        IMAGE_NAME = 'imrans10/my-jenkins-app'
        IMAGE_TAG  = "v${BUILD_NUMBER}"
    }

    stages{
        stage('Clone repo') {
    steps {
        withCredentials([string(credentialsId: 'github-token', variable: 'GH_TOKEN')]) {
            bat 'if exist app rmdir /s /q app'
            bat 'git clone https://%GH_TOKEN%@github.com/Imran-Yoriichi/jenkins-docker-demo app'
        }
    }
}
        

        stage('Build Docker image') {
            steps {
                script {
                   bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% app"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
                    bat "docker tag %IMAGE_NAME%:%IMAGE_TAG% %IMAGE_NAME%:latest"
                    bat "docker push %IMAGE_NAME%:latest"
                }
            }
        }
    }

    post {
        always {
            bat "docker rmi %IMAGE_NAME%:%IMAGE_TAG% || exit 0"
        }
    }
}
