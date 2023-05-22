pipeline {
    agent any
    tools {
        maven 'maven_3_5_0'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nagaraju9951/build-docker-image-jenkins']])
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
