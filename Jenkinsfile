pipeline{
    agent any
    environment {
        DOCKER_IMAGE_NAME = "demo"
        DOCKERFILE_PATH = "Dockerfile"
        DOCKER_IMAGE_TAG = "${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}"
    }
    tools {
        maven 'maven3'
    }
    stages{
        stage('Build Maven') {
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/Su-love/demo-devops']])
                sh "mvn -Dmaven.test.failure.ignore=true clean package"  
            }
        }
        stage('Build Image'){
            steps{  
                script {
                docker.build(DOCKER_IMAGE_TAG, "-f ${DOCKERFILE_PATH} .")
            }
          }
        }
    }
}
