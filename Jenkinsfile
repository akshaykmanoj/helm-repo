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
                bat 'docker build -t  akshaykmanoj/python_registrationimage:v5 . '
                bat 'docker push akshaykmanoj/python_registrationimage:v5'
        }
        }
    }
}
