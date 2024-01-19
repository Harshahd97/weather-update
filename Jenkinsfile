pipeline {
    agent {
        label 'slave2'
    }

    stages {
        stage('checkout') {
            steps {
                sh 'rm -rf weather-update'
                sh 'git clone https://github.com/Harshahd97/weather-update.git'
            }
        }

        stage('build') {
            steps {
                script {
                    sh 'mvn --version'
                    sh 'mvn clean install'
                }
            }
        }
        	
        stage("SonarQube analysis") {
            steps {
                withSonarQubeEnv('sonar') {
                    sh 'mvn clean package sonar:sonar'
              }
            }
        }

        stage('Show Contents of target') {
            steps {
                script {
                    // Print the contents of the target directory
                    sh 'ls -l target'
                }
            }
        }

        stage('Run JAR Locally') {
            steps {
                script {
                    // Run the JAR file using java -jar
                    sh "nohup timeout 10s java -jar target/weather-forecast-app-1.0-SNAPSHOT.jar > output.log 2>&1 &"
                    // Sleep for a while to allow the application to start
                    sleep 10
                }
            }
        }
        
        stage('deploy') {
            steps {
                sh 'ssh root@172.31.28.191'
                sh 'scp /home/slave2/workspace/weather-update_Develop/target/weather-forecast-app-1.0-SNAPSHOT.jar root@172.31.28.191:/opt/apache-tomcat-8.5.98/webapps/'
            }
        }
        
    }
        
    post {
        success {
            echo "Build, Run, and Deployment to Tomcat successful!"
        }
        failure {
            echo "Build, Run, and Deployment to Tomcat failed!"
        }
    }
}
