pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'naga', url: 'https://git.cloudavise.com/visops/t057/jenkinspipeline-docker-image-to-dockerhub']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t jenkins/devops-integration .'
                }
            }
        }
        stage('Push image to Docker Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh "docker login -u nagarajusatyala -p ${dockerhubpwd}"
                        
                        sh 'docker tag jenkins/devops-integration nagarajusatyala/jenkins-docker-image:v1'
                        
                        sh 'docker push nagarajusatyala/jenkins-docker-image:v1'
                    }
                }
            }
        }
    }
}
