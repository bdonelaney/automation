pipeline {
    agent {
        kubernetes {
            label 'functional-test-arunaapp'
            defaultContainer 'curl'
        }
    }
//        stage('Pull source') {
//          checkout scm
//        }
    stages{
        stage('Prepare test') {
            steps {
                //sh 'mvn test'
                sh 'export no_proxy="127.0.0.1, localhost'
                sh 'curl -vL http://localhost:4444/wd/hub/static/resource/hub.html'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}