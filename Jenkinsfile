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
        sh 'cp target/*.jar /opt/springapp/'
    }
}

stage('Stop Old App') {
    steps {
        sh 'pkill -f flipzon.jar || true'
    }
}

stage('Start App') {
    steps {
        sh 'nohup java -jar /opt/springapp/flipzon.jar > app.log 2>&1 &'
    }
}

    }
}
