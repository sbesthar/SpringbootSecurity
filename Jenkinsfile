pipeline {
    agent any

    environment {
        // Replace with your actual Docker Hub username/repository
        DOCKER_IMAGE = "bsivaraj@gmail.com/springboot-security-app"
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

        stage('Maven Build') {
            steps {
                // Generate the .jar file in the target folder
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    // Use the ID of the credentials you created in Jenkins
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'bsivaraj@gmail.com', passwordVariable: 'Sivraj@2098')]) {

                        // 1. Build the image using your Dockerfile in the current directory
                        bat "docker build -t ${DOCKER_IMAGE}:latest ."

                        // 2. Login to Docker Hub
                        bat "echo %PASS% | docker login -u %USER% --password-stdin"

                        // 3. Push to Docker Hub
                        bat "docker push ${DOCKER_IMAGE}:latest"
                    }
                }
            }
        }
    }

    post {
        always {
            // Logout to stay secure and clean the workspace
            bat 'docker logout'
            cleanWs()
        }
    }
}