pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'github-cred',
                        url: 'https://github.com/mehar-pa-45/bus-booking-ui.git'
                    ]]
                )
            }
        }

        stage('Validate') {
            steps {
                sh 'mvn validate'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                echo "Removing old deployment"
                rm -rf /opt/tomcat/webapps/bus-booking*

                echo "Copying new WAR file"
                cp target/bus-booking.war /opt/tomcat/webapps/

                echo "Restarting Tomcat"
                /opt/tomcat/bin/shutdown.sh || true
                sleep 5
                /opt/tomcat/bin/startup.sh
                '''
            }
        }

    }

    post {
        success {
            echo "Application deployed successfully!"
        }
        failure {
            echo "Build or deployment failed!"
        }
    }
}
