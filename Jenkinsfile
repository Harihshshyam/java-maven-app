pipeline {
    agent any

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
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred-id',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh '''
                        echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                        docker push sagar592/java-maven-app:${BUILD_NUMBER}
                        docker tag sagar592/java-maven-app:${BUILD_NUMBER} sagar592/java-maven-app:latest
                        docker push sagar592/java-maven-app:latest
                    '''
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

