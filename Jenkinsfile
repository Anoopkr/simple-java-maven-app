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
                    
                    currentBuildNum = $BUILD_NUMBER
                    while(currentBuildNum){
                        logUrl = 'http://192.168.0.22:8080/job/test/${currentBuildNum}/consoleText'
                        def log = sh(script: 'curl -u ${JENKINS_AUTH} -k ' + logUrl, returnStdout: true).trim()
                        def temp = log.contains('1.0-SNAPSHOT')
                        echo "SAME RELEASE: ${temp}"
                        if(temp){
                            sh """curl -u admin:password -s 'http://192.168.0.22:8080/job/test/${currentBuildNum}/api/xml?wrapper=changes&xpath=//changeSet//comment' >> x.txt"""
                            currentBuildNum = currentBuildNum - 1
                        } else {
                            currentBuildNum = 0
                        } 
                    }    
                    sh 'cat x.txt'
                }
                
            }
        }
    }
//     post{
//         always{
//             cleanWs()
//         }
//     }
}
