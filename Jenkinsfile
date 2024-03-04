pipeline{
    agent any
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
    }
}
