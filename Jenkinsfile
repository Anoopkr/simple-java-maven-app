pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
     environment {
        JENKINS_AUTH = credentials('jenkins_cred')
    }  
  
    stages {
        
        stage('build') {
            steps {
                script{
                 
                    sh 'mvn -B -DskipTests clean package'
                    
                    logUrl = 'http://192.168.0.22:8080/job/test/55/consoleText'
                    def log = sh(script: 'curl -u ${JENKINS_AUTH} -k ' + logUrl, returnStdout: true).trim()
                    def temp = log.contains('1.0-SNAPSHOT')
                    echo "SAME RELEASE: ${temp}"
                    
                    sh """curl -u admin:password -s 'http://192.168.0.22:8080/job/test/55/api/xml?wrapper=changes&xpath=//changeSet//comment' >> x.txt"""
                    sh 'cat x.txt'
                }
                
            }
        }
    }
}
