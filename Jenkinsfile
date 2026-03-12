pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sh 'cp target/hello-webapp.war /opt/homebrew/opt/tomcat@9/libexec/webapps/'
            }
        }
    }
}
