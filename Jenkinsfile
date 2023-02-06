pipeline {
    agent {
    label ('eksdeploy')
    }
    environment {
        dockerImage = ''
        imagename = "uc5-eks"
        registryCredential = 'dockerhub'
    }
    
    stages {
        stage('github') {
            steps {
               git 'https://github.com/akshugithub/UC5-Deploy-to-AWS-EKS-Cluster.git'
            }
        }
		stage('build') {
            steps {
               sh 'mvn clean package'
            }
        }
		stage('build image') {
            steps {
                script {
               withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                 dockerImage = docker.build imagename
                dockerImage.push('latest')
}
                }
            }
        }
        stage('Kubernetes_deployment') {
          steps {
               sh 'kubectl apply -f k8s-deployment.yaml'
               sh 'kubectl apply -f k8s-service.yaml'
            }
        }
    }
}
