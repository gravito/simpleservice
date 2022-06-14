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
                    myapp = docker.build("gravito/simple-service:${env.BUILD_ID}")
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
        stage('Deploy to EKS') {
            steps{
                sh "sed -i 's/simple-service:latest/simple-service:${env.BUILD_ID}/g' kubernetes/app-postgres-deployment.yaml"
                script {
                kubernetesDeploy(configs: "kuberentes/app-postgres-deployment.yaml", kubeconfigId: "kube")
                }
            }
        }
    }    
}
