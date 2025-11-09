pipeline {
    agent any
    
    tools {
        maven "maven399"
    }
    
    stages {
        stage('Fecth code') {
            steps {
                git 'https://github.com/helpingpeopletolearn/MavenBuild.git'
            }
        }
        
        stage('Build Code') {
            steps {
                sh 'mvn clean install'
            }
        }
        
        stage('Deploy code') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat-Id', path: '', url: 'http://localhost:9090')], contextPath: null, war: '**/*.war'
            }
        }
    }
      post {
    success {
      slackSend(
        channel: 'maven-build-channel',
        message: "✅ Build succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        color: 'good'
      )
    }
    failure {
      slackSend(
        channel: 'maven-build-channel',
        message: "❌ Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        color: 'danger'
      )
    }
}
}
