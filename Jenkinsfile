pipeline{
    agent { label 'jenkins-agent' }
    environment {
        DOCKER_IMAGE_NAME = "demo-1.0.0"
        DOCKERFILE_PATH = "Dockerfile"
        DOCKER_IMAGE_TAG = "${DOCKER_IMAGE_NAME}-${BUILD_NUMBER}"
    }
    tools {
        maven 'maven3'
    }
    stages{
        stage('Pull code') {
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Su-love/demo-devops'
            }
        }
        stage('Build Application'){
	    steps{
            	sh "mvn  clean package"  
	    }
	}
        stage('Test Application'){
	    steps{
                sh "mvn test"
           }
	}
       stage("SonarQube Analysis"){
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'sonar-jenkins') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
       }

       stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-jenkins'
                }	
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
