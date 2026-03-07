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
            // 'docker-hub-credentials' is the ID of your credentials in Jenkins
            docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {

                // Build the image using your Docker Hub repository name
                def myImage = docker.build("sbesthar/springboot-security-app:${env.BUILD_ID}")

                // Push the specific build version
                myImage.push()

                // Also tag and push as 'latest'
                myImage.push('latest')
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