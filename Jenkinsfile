pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
        timestamps()
        timeout(time: 20, unit: 'MINUTES')
    }

    parameters {
        booleanParam(name: 'DEPLOY', defaultValue: true, description: 'Deploy to Tomcat after successful build')
        string(name: 'APP_URL', defaultValue: 'http://localhost:8080/hello-webapp', description: 'Application URL for verification')
    }

    environment {
        APP_NAME = 'hello-webapp'
        TOMCAT_HOME = '/opt/homebrew/opt/tomcat@9/libexec'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B clean package'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Backup Existing WAR') {
            when {
                expression { return params.DEPLOY }
            }
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
            when {
                expression { return params.DEPLOY }
            }
            steps {
                sh '''
                    cp "target/${APP_NAME}.war" "$TOMCAT_HOME/webapps/${APP_NAME}.war"
                '''
            }
        }

        stage('Verify Deployment') {
            when {
                expression { return params.DEPLOY }
            }
            steps {
                sh '''
                    curl -fsS "${APP_URL}" > /dev/null
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
