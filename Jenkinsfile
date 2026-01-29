pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        APP_DIR = "/opt/springapp"
        JAR_NAME = "flipzon-0.0.1-SNAPSHOT.jar" // Adjust if Spring Boot renamed it
    }

    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/VenkateshJaggaraju/e-commerceWebApplicationUsingSpringboot.git'
            }
        }
        stage('Prepare App Directory') {
            steps {
                sh '''
                mkdir -p ${APP_DIR}
                chown -R $(whoami) ${APP_DIR}
                chmod -R 755 ${APP_DIR}
                '''
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
    }
}
