pipeline {
    agent any

    environment {
        SERVER_USER = 'tomcat'  // Tomcat user
        SERVER_IP = 'localhost'  // If deploying locally
        DEPLOY_PATH = '/opt/tomcat9/webapps/'  // Tomcat webapps path
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/abdulsamimmondal/java_pipeline', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Add SSH key for SCP and SSH
                withCredentials([sshUserPrivateKey(credentialsId: 'tomcat-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh 'scp -i $SSH_KEY target/*.war $SERVER_USER@$SERVER_IP:$DEPLOY_PATH'
                    sh 'ssh -i $SSH_KEY $SERVER_USER@$SERVER_IP "sudo -S systemctl restart tomcat"'
                }
            }
        }
    }
}
