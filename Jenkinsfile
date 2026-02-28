pipeline {
    agent any

    // Define the environment variables
    environment {
        MAVEN_HOME = tool 'Maven 3.9' // Must match the name in Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulls code from the Git repo you linked in the Jenkins Job
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Runs the Maven wrapper or Maven command to build the JAR
                bat "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
            }
        }

        stage('Unit Test') {
            steps {
                bat "${MAVEN_HOME}/bin/mvn test"
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Saves the generated JAR file so you can download it from Jenkins
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