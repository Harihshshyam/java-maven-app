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
                    // build image into a variable
                    def img = docker.build("sagar592/java-maven-app:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Explicitly log in, tag and push using withRegistry
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-cred-id') {
                        // re‑fetch the image by name and tag
                        def img = docker.image("sagar592/java-maven-app:${BUILD_NUMBER}")
                        img.push()     // push this build number tag
                        img.push('latest') // optional: also push as “latest”
                    }
                }
            }
        }

        // Uncomment once k8s is ready
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

