pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
     environment {
        TEST_VER="123"
        VIVARIUM="/tools/pancake"
        JENKINS_AUTH = credentials('jenkins_cred')
    }  
    stages {
        
        stage('build') {
            steps {
                script{
                 
                    sh 'mvn --version'
                    sh 'mvn -B -DskipTests clean package'
                    // def logFileContent = new File("${JENKINS_HOME}/jobs/${JOB_NAME}/builds/30/consoleText").collect {it}
                    
                    logUrl = 'http://192.168.0.22:8080/job/test/30/consoleText'
                    def log = sh(script: 'curl -u ${JENKINS_AUTH} -k ' + logUrl, returnStdout: true).trim()
                    // def response = sh(script: 'curl -u {JENKINS_AUTH} http://192.168.0.22:8080/job/test/30/consoleText', returnStdout: true)
                    def temp = log.contains('1.0-SNAPSHOT')
                    echo "response: ${log}" 
                    echo "envVar: ${temp}"
                    
                    changeurl = 'http://192.168.0.22:8080/job/test/30/api/xml?wrapper=changes&xpath=//changeSet//comment'
                    def comment = sh(script: 'curl -u ${JENKINS_AUTH} -k ' + changeurl, returnStdout: true).trim()
                    echo "comment: ${comment}"
                    // curl -s "http://192.168.0.22:8080/job/test/30/api/xml?wrapper=changes&xpath=//changeSet//comment"
                }
                
            }
        }
    }
}
