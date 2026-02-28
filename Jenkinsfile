pipeline {
    agent any

    tools {
        // This MUST match the Name exactly in Manage Jenkins -> Tools
        maven 'Maven 3.9.12'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Because of the 'tools' block above, you can just use 'mvn'
                bat 'mvn clean package -DskipTests -U'
            }
        }

        stage('Unit Test') {
            steps {
                bat 'mvn test'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Build process finished.'
        }
        success {
            echo 'Application built successfully!'
        }
        failure {
            echo 'Build failed. Check the console logs.'
        }
    }
}