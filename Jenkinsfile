pipeline {
    agent any
    parameters {
        choice(name: 'DEPLOY_OPTION', choices: ['Deploy any branch', 'Deploy to develop', 'Deploy to master'], description: 'Select deployment option')
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Enter the branch name')
    }
    environment {
        REPO_URL = 'https://github.com/bespalhuk/dockerized.git'
        DEV_CLUSTER = 'dev-cluster'
        PROD_CLUSTER = 'prod-cluster'
    }
    stages {
        stage('--- DETERMINE CLUSTER ---') {
            steps {
                script {
                    if (params.DEPLOY_OPTION == 'Deploy to master') {
                        env.TARGET_CLUSTER = PROD_CLUSTER
                    } else {
                        env.TARGET_CLUSTER = DEV_CLUSTER
                    }
                    echo "Deploying to ${env.TARGET_CLUSTER}"
                }
            }
        }

        stage('--- CHECKOUT CODE ---') {
            steps {
                git branch: params.BRANCH_NAME, url: env.REPO_URL
            }
        }

        stage('--- BUILD ---') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('--- DEPLOY TO MINIKUBE ---') {
            steps {
                script {
                    sh "kubectl config use-context ${env.TARGET_CLUSTER}"
                    sh "kubectl apply -f k8s/deployment.yaml"
                }
            }
        }
    }
}
