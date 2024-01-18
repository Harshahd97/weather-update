pipeline {
    agent { label 'slave2' }
    stages {
        stage('Checkout') {
            steps {
                sh 'rm -rf weather-update'
                sh 'git clone https://github.com/Harshahd97/weather-update.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn --version'
                sh 'mvn clean install'
            }
        }
        stage('Run Locally') {
        steps {
                sh 'java -jar target/weather-forecast-app-1.0-SNAPSHOT.jar &'
                sleep 30
            }
         }
        stage('Deploy') {
            steps {
                sh 'ssh root@172.31.28.191'
                sh 'scp /home/slave2/workspace/parcel_service_pipeline/target/simple-parcel-service-app-1.0-SNAPSHOT.jar root@172.31.28.191:/opt/apache-tomcat-8.5.98/webapps'
        }
        }
    } 
}
