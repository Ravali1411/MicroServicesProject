pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh 'docker build -t ravali2001/loadgenerator:latest .'
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh 'docker push ravali2001/loadgenerator:latest'
                    }
                }
            }
        }

        stage('Deploy to EKS1') {
            steps {
                script {
                    sh '''
                        aws eks --region us-east-1 update-kubeconfig --name EKS1
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                    '''
                }
            }
        }
    }
}
