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
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}