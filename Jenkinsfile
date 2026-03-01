pipeline {
    agent any

    tools {
        // Use the Maven version configured in Jenkins Global Tool Configuration
        maven 'Maven 3.9.12'
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/SpringbootSecurity.git'
            }
        }

        stage('Build Image Tar') {
            steps {
                // We use buildTar to create the file in the target folder
                // -DskipTests avoids the surefire plugin version issues
                bat 'mvn compile jib:buildTar -DskipTests'
            }
        }

        stage('Load Image to Docker') {
            steps {
                // Load the tar file into the local Docker daemon on the Jenkins agent
                bat 'docker load --input target/jib-image.tar'
            }
        }

        stage('Run Container') {
            steps {
                // Stop and remove old container if it exists, then run the new one
                bat '''
                    docker stop spring-app || true
                    docker rm spring-app || true
                    docker run -d --name spring-app -p 8080:8080 springboot-security-app
                '''
            }
        }
    }

    post {
        always {
            // Clean up the workspace to save disk space
            cleanWs()
        }
    }
}