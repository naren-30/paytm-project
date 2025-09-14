pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "naren3005/paytm"
        DOCKER_CREDENTIALS_ID = "docker"  // Jenkins Credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE}:latest .'
                }
            }
        }

        stage('Run Container for Test') {
            steps {
                script {
                    // Stop old container if running
                    sh 'docker rm -f paytm-static-test || true'
                    
                    // Run container on port 8080
                    sh 'docker run -d --name paytm-static-test -p 8086:80 ${DOCKER_IMAGE}:latest'
                    
                    // Quick test (optional)
                    sh 'curl -I http://localhost:8086 || true'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: "DOCKER_USER", passwordVariable: "DOCKER_PASS")]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:latest
                        docker logout
                    '''
                }
            }
        }
    }
}

