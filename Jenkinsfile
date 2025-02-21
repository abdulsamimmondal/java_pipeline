pipeline {
    agent any

    environment {
        SERVER_USER = 'tomcat'  // Change to your Tomcat server user
        SERVER_IP = 'localhost'  // Change this if deploying remotely
        DEPLOY_PATH = '/opt/tomcat9/webapps/'  // Change if your Tomcat path is different
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url:'https://github.com/KPkm25/java_jenkins', branch:'main'
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
        sh 'scp target/*.war tomcat@localhost:/opt/tomcat9/webapps/'
        sh 'ssh tomcat@localhost "sudo -S systemctl restart tomcat"'
    }
}

    }
}
