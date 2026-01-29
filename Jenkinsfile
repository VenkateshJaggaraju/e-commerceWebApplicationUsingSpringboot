pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        APP_SERVER_IP = "172.31.17.216"
        APP_USER = "ubuntu"
        APP_DIR = "/opt/springapp"
        JAR_NAME = "flipzon.jar"
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
                sh "cp target/${env.JAR_NAME} ${env.APP_DIR}/"
            }
        }

        stage('Stop Old App') {
            steps {
                sh "pkill -f ${env.JAR_NAME} || true"
            }
        }

        stage('Start App') {
            steps {
                sh "nohup java -jar ${env.APP_DIR}/${env.JAR_NAME} > ${env.APP_DIR}/app.log 2>&1 &"
            }
        }
    }
}
