pipeline {
    agent any
    
   environment {
        MAVEN_HOME = tool 'maven'
    }
    stages {
        stage('GitHub Checkout') {
            steps {
                git branch: 'main', credentialsId: 'Jenkins', url: 'https://github.com/mova-git/jenkins.git'
            }
        }
        stage('Build with Maven') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }
    }
}
