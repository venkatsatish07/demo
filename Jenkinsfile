pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/venkatsatish07/demo', branch: 'main'
            }
        }
        stage('Build with Maven') {
            steps {
                script {
                    docker.image('maven:3.6.3-jdk-11').inside {
                        sh 'mvn clean package'
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def jdImage = "venkats061/demo:latest"
                    sh "docker build -t ${jdImage} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        sh "docker push ${jdImage}"
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
