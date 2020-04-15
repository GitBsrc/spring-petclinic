pipeline {
    agent any
    tools {
        maven 'Apache 3.6.3'
    }
    stages {
        stage('Build') {
            steps {
                slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
                withEnv( ["PATH+MAVEN=Apache 3.6.3/bin"] ) {
                    bat 'mvn clean' 
                }
            }
        }
        stage('Test') {
            steps {
                withEnv( ["PATH+MAVEN=Apache 3.6.3/bin"] ) {
                    bat 'mvn test' 
                }
            }
        }
        stage('Package') {
            steps {
                withEnv( ["PATH+MAVEN=Apache 3.6.3/bin"] ) {
                    bat 'mvn package' 
                }
            }
        }
        stage('Deploy') {
            when {
            expression {
                GIT_BRANCH = sh(returnStdout: true, script: 'git rev-parse --abbrev-ref HEAD').trim()
                return (GIT_BRANCH == 'master')
                }
            }
            steps {
                withEnv( ["PATH+MAVEN=Apache 3.6.3/bin"] ) {
                    bat 'mvn deploy' 
                }
            }
        }
    }
    post {
        success{
            slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure{
           slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})") 
        }
    }
}
