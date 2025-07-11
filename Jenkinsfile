pipeline {
    agent any

    tools {
        // Ensure Maven is installed and configured in Global Tool Configuration
        maven 'Maven 3.8.1'
        // Make sure JDK 17 (or your version) is configured there as well
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest code from GitHub
                git url: 'https://github.com/Harihshshyam/java-maven-app.git', branch: 'main'
            }
        }

        stage('Compile & Test') {
            steps {
                // Compile, run tests, and fail the build if tests fail
                sh 'mvn clean compile test'
            }
            post {
                always {
                    // Publish JUnit test reports
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                // Package into a JAR (skips tests)
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive Artifact') {
            steps {
                // Save the built JAR in Jenkins so you can download it later
                archiveArtifacts artifacts: 'target/java-maven-app-*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build succeeded!'
        }
        failure {
            echo '❌ Build failed. Check the console output for errors.'
        }
    }
}

