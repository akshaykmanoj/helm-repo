pipeline {
    agent any
    environment {
        build_number = "${env.BUILD_ID}"
        AWS_ACCOUNT_ID="409486179793"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="registration-helm"
        //IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
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
        // stage('Update Helm value file') {
        //     steps {
        //         script {
        //             def buildNumber = env.BUILD_NUMBER
        //             def valueFilePath ='./registration-helm/values.yaml'
        //             def imageTag = "v${buildNumber}"
            
        //     // Update the image tag in the Helm value file
        //             bat "sed -i 's|tag: \".*\"|tag: \"${imageTag}\"|' ${valueFilePath}"
        //         }
        //     }
        // }
        stage('helmChart tag and  push to ECR') {
            steps {
                // sh "sed -i 's|sreekanthpv12/nodebackend:v5|sreekanthpv12/nodebackend:${build_number}|g' ./node-app-chart/values.yaml"
                sh "sed -i 's|akshaykmanoj/python_registrationimage:v5|akshaykmanoj/python_registrationimage:${env.BUILD_NUMBER}|g' ./registration-helm/values.yaml"
            }
        }
        
        stage('helm package '){
            steps {
                sh "helm package registration-helm "
            }
        }
    }
}
