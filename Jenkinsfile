pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk17'
    }

    environment {
        APP_SERVER_IP = "172.31.38.20"
        APP_USER = "ubuntu"
        APP_DIR = "/opt/springapp"
        JAR_NAME = "springboot-app.jar"
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

        stage('Copy JAR to App Server') {
            steps {
                sh """
                scp target/${JAR_NAME} ${APP_USER}@${APP_SERVER_IP}:${APP_DIR}/
                """
            }
        }

        stage('Stop Old App') {
            steps {
                sh """
                ssh ${APP_USER}@${APP_SERVER_IP} '
                pkill -f ${JAR_NAME} || true
                '
                """
            }
        }

        stage('Start New App') {
            steps {
                sh """
                ssh ${APP_USER}@${APP_SERVER_IP} '
                nohup java -jar ${APP_DIR}/${JAR_NAME} > app.log 2>&1 &
                '
                """
            }
        }
    }

    post {
        success {
            echo "✅ Spring Boot App Deployed Successfully"
        }
        failure {
            echo "❌ Deployment Failed"
        }
    }
}
