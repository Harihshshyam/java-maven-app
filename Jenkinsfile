pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred-id')
    }

    stages {
        stage('Build Maven App') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("sagar592/java-maven-app:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
                        docker.image("sagar592/java-maven-app:${BUILD_NUMBER}").push()
                    }
                }
            }
        }

        // stage('Deploy to Kubernetes') {
        //     steps {
        //         sh 'kubectl apply -f k8s/deployment.yaml'
        //         sh 'kubectl apply -f k8s/service.yaml'
        //     }
        // }
    }

    post {
        success {
            echo "✅ Build and Push Successful!"
        }
        failure {
            echo "❌ Build or Push Failed"
        }
    }
}

