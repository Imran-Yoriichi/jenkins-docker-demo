pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'echo Building on branch: %GIT_BRANCH%'
            }
        }

        stage('Deploy to dev') {
            when {
                branch 'dev'
            }
            steps {
                bat 'echo Deploying to STAGING environment...'
            }
        }

        stage('Deploy to production') {
            when {
                branch 'main'
            }
            steps {
                bat 'echo Deploying to PRODUCTION environmenttt...'
            }
        }
    }

    post {
        success {
            bat 'echo Pipeline succeeded on %GIT_BRANCH%'
        }
        failure {
            bat 'echo Pipeline failed on %GIT_BRANCH%'
        }
    }
}
