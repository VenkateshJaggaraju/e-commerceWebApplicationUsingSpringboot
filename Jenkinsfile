pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        APP_SERVER_IP = "172.31.41.238"
        APP_USER = "ubuntu"
        APP_DIR = "/opt/springapp"
        JAR_NAME = "yourapp.jar"
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

        stage('Copy JAR') {
            steps {
                sh "scp target/${JAR_NAME} ${APP_USER}@${APP_SERVER_IP}:${APP_DIR}/"
            }
        }

        stage('Stop Old App') {
            steps {
                sh "ssh ${APP_USER}@${APP_SERVER_IP} 'pkill -f ${JAR_NAME} || true'"
            }
        }

        stage('Start App') {
            steps {
                sh "ssh ${APP_USER}@${APP_SERVER_IP} 'nohup java -jar ${APP_DIR}/${JAR_NAME} &'"
            }
        }
    }
}
