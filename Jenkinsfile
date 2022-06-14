pipeline {
    agent any

    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("DOCKER-HUB-USERNAME/simple-service:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                            docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/simple-service:latest/simple-service:${env.BUILD_ID}/g' kubernetes/app-postgres-deployment.yaml"
                sh "kubectl apply -f kubernetes/app-postgres-deployment.yaml"
                sh "kubectl rollout restart deployment fullstack-app-postgres"
            }
        }
    }    
}
