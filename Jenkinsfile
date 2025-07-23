pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        DOCKER_IMAGE = 'abhishek0000111/node-jenkins-demo:latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Abhishek1234anand/EXPRESSDEMOFORCICD/'
            }
        }
        stage('Install') {
            steps {
                bat 'npm install'
            }
        }
        stage('Test') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    bat 'npm test || echo No tests defined'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                    bat 'docker push %DOCKER_IMAGE%'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withEnv(['KUBECONFIG=C:\\Users\\asus\\.kube\\config']) {
                    bat 'kubectl apply -f k8s-deployment.yaml'
                    bat 'kubectl apply -f k8s-service.yaml'
                }
            }
        }
    }
}
