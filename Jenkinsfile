pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean verify'
            }
        }
        stage('Package') {
            steps {
                bat 'mvn package'
                bat 'docker build -t cladiomartins/javaeedocker .'
            }
        }
        stage('Deploy') {
            steps {
                bat 'docker rm -f javaeedocker || true'
                bat 'docker run -d --name javaeedocker -p 8080:8080 -p 4848:4848 cladiomartins/javaeedocker'
            }
        }
        stage('Slack') {
            steps {
                slackSend teamDomain: 'playground', channel: '#having-fun', color: 'good', message: 'Lets go back to have fun'
            }
        }
    }
}