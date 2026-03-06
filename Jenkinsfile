pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username/repository
        DOCKER_IMAGE = 'bsivaraj@gmail.com/springboot-security-app'
    }

    tools {
        maven 'Maven 3.9.12'
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sbesthar/SpringbootSecurity.git'
            }
        }

        stage('Build and Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'bsivaraj@gmail.com', passwordVariable: 'Sivraj@2098')]) {
                    // Jib pushes directly using the provided credentials
                    bat "mvn compile jib:build -Dimage=${DOCKER_IMAGE} -Djib.to.auth.username=${DOCKER_USER} -Djib.to.auth.password=${DOCKER_PASS} -DskipTests"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}