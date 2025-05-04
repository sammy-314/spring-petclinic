pipeline {
    agent any
    tools {
        maven 'Maven' // Use a generic name; we'll configure it in Jenkins
    }
    environment {
        DOCKER_IMAGE = "sammytrips/spring-petclinic:${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/sammy-314/spring-petclinic.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-creds', url: 'https://index.docker.io/v1/']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl set image deployment/petclinic-deployment petclinic-container=$DOCKER_IMAGE'
            }
        }
    }
}