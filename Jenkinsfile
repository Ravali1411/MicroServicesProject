pipeline {
    agent any

    stages {
        stage('Deploy To Kubernetes') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS1', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://AA088BF71B09FD232F54CB9B70BACB00.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl apply -f deployment-service.yml"
                    
                }
            }
        }
        
        stage('verify Deployment') {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'EKS1', contextName: '', credentialsId: 'k8-token', namespace: 'webapps', serverUrl: 'https://AA088BF71B09FD232F54CB9B70BACB00.gr7.us-east-1.eks.amazonaws.com']]) {
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}
