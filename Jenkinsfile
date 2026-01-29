pipeline {
    agent any

    tools {
        maven 'maven' // Make sure this matches your Jenkins Maven tool name
    }

    environment {
        APP_SERVER_IP = "172.31.17.216"
        APP_USER = "ubuntu"
        APP_DIR = "/opt/springapp"
        JAR_NAME = "flipzon.jar"
        SSH_KEY = "/home/jenkins/.ssh/id_rsa" // Path to Jenkins private key
    }

    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/VenkateshJaggaraju/e-commerceWebApplicationUsingSpringboot.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Copy JAR to Remote Server') {
            steps {
                sh """
                    scp -i ${SSH_KEY} target/${JAR_NAME} ${APP_USER}@${APP_SERVER_IP}:${APP_DIR}/
                """
            }
        }

        stage('Stop Old App on Remote') {
            steps {
                sh """
                    ssh -i ${SSH_KEY} ${APP_USER}@${APP_SERVER_IP} 'pkill -f ${JAR_NAME} || true'
                """
            }
        }

        stage('Start App on Remote') {
            steps {
                sh """
                    ssh -i ${SSH_KEY} ${APP_USER}@${APP_SERVER_IP} 'nohup java -jar ${APP_DIR}/${JAR_NAME} > ${APP_DIR}/app.log 2>&1 &'
                """
            }
        }
    }
}
