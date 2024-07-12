pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/your-username/newme-shopee.git'
        IMAGE_NAME = 'newme-shopee'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: REPO_URL
            }
        }
        stage('Build') {
            steps {
                script {
                    dockerImage = docker.build(IMAGE_NAME)
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'npm test'
                    }
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
        stage('Deploy to AWS') {
            steps {
                // Add deployment steps (e.g., ECS, EKS, EC2)
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
