pipeline {
    agent any

    tools {
        maven 'maven'
    }

    parameters {
        booleanParam(name: 'RUN_APP', defaultValue: true, description: 'Check to start Spring Boot app')
    }

    environment {
        APP_DIR = "/opt/springapp"
        JAR_NAME = "flipzon.jar"      
        APP_SERVER_IP = "172.31.17.216"  
        DB_SERVER_IP = "172.31.7.153"  
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

        stage('Copy JAR') {
            steps {
                sh "cp target/${JAR_NAME} ${APP_DIR}/"
            }
        }

        stage('Stop Old App') {
            steps {
                sh '''
                pkill -f ${JAR_NAME} || true
                '''
            }
        }

        stage('Start App') {
            when {
                expression { return params.RUN_APP }
            }
            steps {
                sh "nohup java -jar ${APP_DIR}/${JAR_NAME} > ${APP_DIR}/app.log 2>&1 &"
                echo "Spring Boot app started successfully!"
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
        stage('Start App') {
            when {
                expression { 
                    return params.RUN_APP
                }
            }
            steps {
                sh """
                nohup java -jar ${APP_DIR}/${JAR_NAME} \
                --server.port=8081 \
                --server.address=0.0.0.0 \
                --spring.datasource.url=jdbc:mysql://172.31.7.153:3306/flipzone \
                --spring.datasource.username=admin \
                --spring.datasource.password=Hamsi@123 \
                > ${APP_DIR}/app.log 2>&1 &
                """
                echo "Spring Boot app started successfully on 65.0.67.175:8081!"
            }
        }
    }
}
