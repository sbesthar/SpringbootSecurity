pipeline {
    agent any

    environment {
        // Your Docker Hub Repository
        DOCKER_REPO = "bsivaraj@gmail.com/springboot-security-app"
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
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    // Authenticate with Docker Hub using your Jenkins Credentials ID
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {

                        // Build the image locally on the Jenkins agent
                        def myImage = docker.build("${env.DOCKER_REPO}:${env.BUILD_ID}")

                        // Push the specific Build ID tag and the 'latest' tag
                        myImage.push()
                        myImage.push('latest')
                    }
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