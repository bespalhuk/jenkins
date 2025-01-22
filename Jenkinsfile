pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', credentialsId: 'bespalhuk', url: 'https://github.com/bespalhuk/jenkins.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying the project...'
            }
        }
    }
}
