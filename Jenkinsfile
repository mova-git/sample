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
                sh 'mv target/*.jar target/app.jar'
            }
        }
        stage('Deploy to Remote Server') {
        steps {
            echo 'Deploying to remote server...'

            // Make sure your private key is configured in Jenkins Credentials (ID: my-ssh-key)
            sshagent(['my-ssh-key']) {

            // Step 1: Install Apache HTTPD
            sh '''
            ssh -o StrictHostKeyChecking=no ubuntu@65.1.130.231 '
              sudo apt update &&
              sudo apt install -y apache2
            '
            '''

            // Step 2: Copy the JAR to the remote server
            sh '''
            scp -o StrictHostKeyChecking=no target/app.jar ubuntu@65.1.130.231:/tmp/app.jar
            '''

            // Step 3: Move JAR to Apache path (usually not ideal, but for demo it’s fine)
            sh '''
            ssh ubuntu@65.1.130.231 '
              sudo mv /tmp/app.jar /var/www/html/app.jar
            '
            '''
        }
    }
}
}
}
