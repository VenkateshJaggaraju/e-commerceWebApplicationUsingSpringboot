pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
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
                sh "cp target/${JAR_NAME} ${APP_DIR}/"
            }
        }

        stage('Stop Old App') {
            steps {
                sh "pkill -f ${JAR_NAME} || true"
            }
        }

        stage('Start App') {
            steps {
                sh "nohup java -jar ${APP_DIR}/${JAR_NAME} > ${APP_DIR}/app.log 2>&1 &"
            }
        }
    }
}
