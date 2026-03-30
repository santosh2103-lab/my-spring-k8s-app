pipeline {
    agent any
    environment {
        IMAGE_NAME = "my-spring-app"
        TAG = "${env.BUILD_ID}"
    }
    stages {
        stage('Build Jar') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('Docker Build & Tag') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${TAG} ."
                sh "docker tag ${IMAGE_NAME}:${TAG} ${IMAGE_NAME}:latest"
            }
        }
        stage('Deploy to K8s') {
            steps {
                // Docker Desktop context is usually already set
                sh "kubectl apply -f k8s/deployment.yaml"
                sh "kubectl set image deployment/spring-app-deploy spring-app-container=${IMAGE_NAME}:${TAG}"
            }
        }
    }
}