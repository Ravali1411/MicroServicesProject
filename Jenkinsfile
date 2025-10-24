pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ravali2001/adservice:latest"
        DOCKER_CREDENTIALS = "docker-cred"
        AWS_REGION = "us-east-1"
        EKS_CLUSTER = "EKS1"
    }

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: env.DOCKER_CREDENTIALS) {
                        sh "docker build -t ${env.DOCKER_IMAGE} ."
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: env.DOCKER_CREDENTIALS) {
                        sh "docker push ${env.DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                    sh "aws eks --region ${env.AWS_REGION} update-kubeconfig --name ${env.EKS_CLUSTER}"

                    def yamls = ['deployment-service.yml', 'service-account.yml', 'role.yml', 'secret.yml']

                    for (file in yamls) {
                        if (!fileExists(file)) {
                            error "Deployment failed: Required file '${file}' is missing in workspace"
                        }
                    }

                    for (file in yamls) {
                        sh "kubectl apply -f ${file}"
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    sh "kubectl get svc"
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully"
        }
        failure {
            echo "Deployment failed"
        }
    }
}
