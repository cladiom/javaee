def getBuildUser() {
    return currentBuild.rawBuild.getCause(Cause.UserIdCause).getUserId()
}

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
            script {
                BUILD_USER = getBuildUser()
            }
           slackSend teamDomain: 'playground', channel: '#having-fun', color: 'good', message: "*${currentBuild.currentResult}:* ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${BUILD_USER}\n More info at: ${env.BUILD_URL} "
        }
        failure {
            script {
                BUILD_USER = getBuildUser()
            }
           slackSend teamDomain: 'playground', channel: '#having-fun', color: 'danger', message: "*${currentBuild.currentResult}:* ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${BUILD_USER}\n More info at: ${env.BUILD_URL} "
        }
        //always {
           //slackSend teamDomain: 'playground', channel: '#having-fun', color: 'good', message: 'Lets go back to have fun'
        //}
        
    }
}