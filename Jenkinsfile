pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh 'docker build -t ravali2001/adservice:latest .'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh 'docker push ravali2001/adservice:latest'
                    }
                }
            }
        }

        stage('Deploy to EKS1') {
            steps {
                script {
                    sh '''
                        aws eks --region us-east-1 update-kubeconfig --name EKS1
                        kubectl apply -f deployment-service.yml
                        kubectl apply -f service-account.yml
                        kubectl apply -f role.yml
                        kubectl apply -f secret.yml
                    '''
                }
            }
        }
    }
}
