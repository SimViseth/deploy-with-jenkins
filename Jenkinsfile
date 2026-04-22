pipeline {
    agent any

    stages{

        stage('Build Gradle'){
            steps{
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/SimViseth/deploy-with-jenkins']]
                ])

                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -t viseth27/devops-integration .'
                }
            }
        }

        stage('Push Image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u viseth27 -p ${dockerhubpwd}'
                    }
                    sh 'docker push viseth27/devops-integration'
                }
            }
        }
    }
}