pipeline {
    agent any

    stages {
        stage ('1. Checkout code') {
            steps {
                checkout scm
            }
        }
        stage ('2. Build Docker Image') {
            steps {
                sh 'docker build -t sriramsrb/aura-shopping:latest .'
            }
        }
        stage ('3. Push docker image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub', variable: 'DOCKER_PWD')]) {
                    sh 'echo "$DOCKER_PWD" | docker login -u sriramsrb --password-stdin'
                    sh 'docker push sriramsrb/aura-shopping:latest'
                }
            }
        }
        stage ('4. Deploy to kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl rollout restart deployment aura-shopping-deployment'
            }
        }
    }
}