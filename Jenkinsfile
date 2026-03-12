pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
        timeout(time: 20, unit: 'MINUTES')
    }

    environment {
        APP_NAME = 'hello-webapp'
        TOMCAT_HOME = '/opt/homebrew/opt/tomcat@9/libexec'
        APP_URL = 'http://localhost:8080/hello-webapp'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B clean package'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.war'
            }
        }

        stage('Backup Existing WAR') {
            steps {
                sh '''
                    mkdir -p backups
                    if [ -f "$TOMCAT_HOME/webapps/${APP_NAME}.war" ]; then
                      cp "$TOMCAT_HOME/webapps/${APP_NAME}.war" "backups/${APP_NAME}-${BUILD_NUMBER}.war"
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    cp "target/${APP_NAME}.war" "$TOMCAT_HOME/webapps/${APP_NAME}.war"
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    curl -fsS "$APP_URL" > /dev/null
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the console output.'
        }
        always {
            echo "Finished build #${BUILD_NUMBER}"
        }
    }
}
