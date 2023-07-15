pipeline {
    agent any
    tools {
        Python 'Python'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_key_helm', url: 'https://github.com/akshaykmanoj/helm-repo.git']])
            }

        stage('docker build and push')  {
        bat 'docker build -t  akshaykmanoj/python_registrationimage:v5 '
        }
    
}
