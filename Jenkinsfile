pipeline {
    agent any

    tools {
        jdk 'jdk17'
    }

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
                    docker.image('docker:24.0.5').inside {
                        sh 'docker build -t viseth27/devops-integration .'
                    }
                }
            }
        }

        stage('Push Image to Hub'){
            steps{
                script{
                    docker.image('docker:24.0.5').inside {
                        withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                            sh 'docker login -u viseth27 -p ${dockerhubpwd}'
                        }
                        sh 'docker push viseth27/devops-integration'
                    }
                }
            }
        }
    }
}