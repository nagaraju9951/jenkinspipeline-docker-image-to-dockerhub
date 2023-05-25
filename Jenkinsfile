pipeline {
    agent {
        node {
           label 't057-runner' 
        }
    }
    
    tools {
        maven 'maven'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'ca-git-access', url: 'https://git.cloudavise.com/visops/t057/jenkinspipeline-docker-image-to-dockerhub.git']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'sudo docker build -t devopss/devops-integration .'
                }
            }
        }
        stage('Push image to Docker Hub') {
            steps {
                script {
                       
                        withCredentials([usernamePassword(credentialsId: 't57-new', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {

                        sh "echo ${DOCKER_HUB_PASSWORD} |sudo docker login --username ${DOCKER_HUB_USERNAME} --password-stdin"
                       
                        sh 'sudo docker tag devopss/devops-integration nagarajusatyala/jenkins-docker-image:L1'
                        
                        sh 'sudo docker push nagarajusatyala/jenkins-docker-image:L1'
                    }
                }
            }
        }

    }
}