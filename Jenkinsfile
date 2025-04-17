pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'fetake/springdemo'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/FETAKE/springdemo.git'
            }
        }

        stage('Checkout') {
            steps {
                git url: 'https://github.com/FETAKE/springdemo.git', branch: 'main', credentialsId: 'github-creds'
            }
        }

        stage('Build App') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 8085:8080 --name springboot-app $IMAGE_NAME'
            }
        }
    }
}
