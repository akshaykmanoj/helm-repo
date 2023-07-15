pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_key_helm', url: 'https://github.com/akshaykmanoj/helm-repo.git']])
            }
        }

        stage('docker login build push and logout')  {
            steps{
                withCredentials([string(credentialsId: 'akshaykmanoj', variable: 'docker-pwd')]) {
                    bat 'docker login -u akshaykmanoj -p %docker-pwd%'
                    bat "docker build -t  akshaykmanoj/python_registrationimage:${env.BUILD_NUMBER} . "
                    bat "docker push akshaykmanoj/python_registrationimage:${env.BUILD_NUMBER}"
                    bat 'docker logout'
                }
            }
        }
        stage('Update Helm value file') {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    def valueFilePath ='registration-helm/values.yaml'
                    def imageTag = "v${buildNumber}"
            
            // Update the image tag in the Helm value file
                    bat "sed -i 's|tag: \".*\"|tag: \"${imageTag}\"|' ${valueFilePath}"
                }
            }
        }
    }
}
