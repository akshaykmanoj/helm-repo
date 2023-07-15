pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_key_helm', url: 'https://github.com/akshaykmanoj/helm-repo.git']])
            }
        }

        stage('docker build and push')  {
            steps{
                withCredentials([string(credentialsId: 'akshaykmanoj', variable: 'docker-pwd')]) {
                    bat 'docker login -u akshaykmanoj -p %docker-pwd%'
                    bat 'docker build -t  akshaykmanoj/python_registrationimage:v5 . '
                    bat 'docker push akshaykmanoj/python_registrationimage:${env.BUILD_NUMBER}'
                }
            }
        }
    }
}
