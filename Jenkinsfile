pipeline {
    agent any
    
    environment {
        JAVA_HOME = '/usr/lib/jvm/java-17-openjdk-amd64' // Adjusted JDK path for Java 17
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/venkatsatish07/demo', branch: 'main'
            }
        }
        
        stage('Build with Maven') {
            steps {
                script {
                    docker.image('maven:3.8.4-openjdk-17').inside {
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
                    currentBuild.displayName = "# $jdImage"
                    currentBuild.description = "Pushed $jdImage to Docker Hub"
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    def jdImage = "venkats061/demo:latest"
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
