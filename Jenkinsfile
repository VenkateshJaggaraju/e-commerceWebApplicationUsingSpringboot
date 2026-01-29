pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'jdk17' // Make sure this matches your Jenkins JDK tool name
    }

    environment {
        APP_DIR = "/opt/springapp"
        JAR_NAME = "flipzon-0.0.1-SNAPSHOT.jar" // adjust if needed
        APP_SERVER_IP = "172.31.17.216"         // your app server private IP
        DB_SERVER_IP = "172.31.20.50"           // your DB server private IP
    }

    stages {

        stage('Prepare App Directory') {
            steps {
                sh '''
                mkdir -p ${APP_DIR}
                chown -R $(whoami) ${APP_DIR}
                chmod -R 755 ${APP_DIR}
                '''
            }
        }

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

        stage('Verify JAR') {
            steps {
                sh 'ls -l target/'
            }
        }

        stage('Copy JAR') {
            steps {
                sh "cp target/${JAR_NAME} ${APP_DIR}/"
            }
        }

        stage('Stop Old App') {
            steps {
                sh '''
                if pgrep -f ${JAR_NAME}; then
                    pkill -f ${JAR_NAME}
                    echo "Old app stopped"
                else
                    echo "No running app found"
                fi
                '''
            }
        }

        stage('Start App') {
            steps {
                sh "nohup java -jar ${APP_DIR}/${JAR_NAME} > ${APP_DIR}/app.log 2>&1 &"
            }
        }

        stage('Show Server IPs') {
            steps {
                sh '''
                echo "App Server Private IP: ${APP_SERVER_IP}"
                echo "DB Server Private IP: ${DB_SERVER_IP}"
                '''
            }
        }
    }
}
