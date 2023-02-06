pipeline {
    agent {
    label ('docker')
    }
    environment {
        dockerImage = ''
        imagename = "<image_name>"
        registryCredential = 'dockerhub'
    }
    
    stages {
        stage('scm') {
            steps {
               git '<SCM>'
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
               sh 'kubectl apply -f Kubernetes_Deployment.yaml'
               sh 'kubectl apply -f Kubernetes_Service.yaml'
            }
        }
    }
}
