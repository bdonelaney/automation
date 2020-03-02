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
    stage('Prepare test') {
        container('jnlp-agent') {
            //requires mysql
            try {
                sh 'mvn test'
            } finally {
                junit 'target/surefire-reports/*.xml'
            }
        }
    }
}