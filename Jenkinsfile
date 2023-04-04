pipeline {
    agent any
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/S13101997/devops-automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t shrutz/devops-integration .'
                }
            }
        }
        stage('Push Image'){
            steps {
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerpwd')]) {
                    sh 'docker login -u shrutz -p ${dockerpwd}'
                }
                sh 'docker push shrutz/devops-integration'
            }
        }
    }
    post {
       always {
          junit(
               allowEmptyResults: true,
               testResults: '*/test-reports/.xml'
            )
      }
   }
}
