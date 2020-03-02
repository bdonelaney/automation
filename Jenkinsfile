podTemplate(
  name: 'functional-test-arunaapp',
  label: 'functional-test-arunaapp',
  cloud: 'kubernetes',
  containers: [
    //Java agent, test executor
    containerTemplate(name: 'jnlp-agent',
                      image: 'etlabvlldvopap2.et.lab:80/docker/mavenjnlp:latest',
                      resourceLimitMemory: '512Mi',
                      workingDir: '/home/jenkins/agent',
                      command: '/bin/sh -c',
                      //args: '"umask 0000; /usr/local/bin/run-jnlp-client ${computer.jnlpmac} ${computer.name}"',
                      args: 'cat',
                      imagePullSecrets: ["artifactorydocker"],
                      alwaysPullImage: false,
                      envVars: [
                        //Heap for jnlp is 1/2, for mvn and surefire process is 1/4 of resourceLimitMemory by default
                        envVar(key: 'JNLP_MAX_HEAP_UPPER_BOUND_MB', value: '64'),
                        envVar(key: 'JENKINS_URL', value: 'http://10.254.37.109:80'),
                        envVar(key: 'JENKINS_URL', value: '10.254.37.109:50000')
                      ]),
    //App under test
    containerTemplate(name: 'arunaapp',
                      image: 'etlabvlldvopap2.et.lab:80/docker/arunatest:testcandidate',
                      resourceLimitMemory: '512Mi',
                      alwaysPullImage: true,
                      imagePullSecrets: ["artifactorydocker"],
                      envVars: [
                        envVar(key: 'SPRING_PROFILES_ACTIVE', value: 'k8sit'),
                        envVar(key: 'SPRING_CLOUD_KUBERNETES_ENABLED', value: 'false')
                      ]),
    containerTemplate(name: 'selenium-chrome',
                        image: 'selenium/standalone-chrome',
                        resourceLimitMemory: '512Mi',
                        alwaysPullImage: false
    )
  ]
)
{
    node('functional-test-arunaapp') {
        stage('Pull source') {
          checkout scm
        }
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
}