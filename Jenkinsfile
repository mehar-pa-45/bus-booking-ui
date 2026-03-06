pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        credentialsId: 'github-cred',
                        url: 'https://github.com/mehar-pa-45/bus-booking-ui.git'
                    ]]
                )
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                echo "Removing old deployment"
                rm -rf /opt/tomcat/webapps/bus-booking*

                echo "Copying new WAR"
                cp target/bus-booking.war /opt/tomcat/webapps/
                '''
            }
        }
    }
}
