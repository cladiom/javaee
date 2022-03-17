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
    }
    post {
        success {
           slackSend teamDomain: 'playground', channel: '#having-fun', color: 'good', message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${env.GIT_COMMITTER_NAME}\n More info at: ${env.BUILD_URL}"
        }
        failure {
           slackSend teamDomain: 'playground', channel: '#having-fun', color: 'danger', message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${env.GIT_COMMITTER_NAME}\n More info at: ${env.BUILD_URL}"
        }
        //always {
           //slackSend teamDomain: 'playground', channel: '#having-fun', color: 'good', message: 'Lets go back to have fun'
        //}
        
    }
}