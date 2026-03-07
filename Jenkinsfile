pipeline {
    agent any

    environment {
        // Updated to your Docker Hub username
        DOCKER_REPO = "sbesthar/springboot-security-app"
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
                    // This block automatically handles 'docker login' and 'docker logout'
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        // Build using the environment variable and build ID
                        def myImage = docker.build("${DOCKER_REPO}:${env.BUILD_ID}")

                        // Push the specific build version
                        myImage.push()

                        // Also tag and push as 'latest'
                        myImage.push('latest')
                    }
                }
            }
        }
    } // <--- This was the missing closing brace for 'stages'

    post {
        always {
            // docker.withRegistry already logged out, so we just clean the workspace
            cleanWs()
        }
    }
}