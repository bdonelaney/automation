pipeline {
    agent {
        kubernetes {
            label 'functional-test-arunaapp'
            defaultContainer 'jnlp-agent'
        }
    }
//        stage('Pull source') {
//          checkout scm
//        }
    stages{
        stage('Prepare test') {
            //requires mysql
            try {
                sh 'mvn test'
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
        }
    }
}