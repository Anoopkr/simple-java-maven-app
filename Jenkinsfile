pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
     environment {
        JENKINS_AUTH = credentials('jenkins_cred')
        VERSION = readMavenPom().getVersion()
    }  
  
    stages {
        
        stage('build') {
            steps {
                script{
                    echo "VERSION: ${VERSION}"
                    sh 'mvn -B -DskipTests clean package'
                    
                    Jenkins.instance.getItem("test").each{

			            def jobBuilds=it.getBuilds()

			        	jobBuilds.each { build ->
            				def currentStatus = build.buildStatusSummary.message                            
            				def log = build.log
            				def isCurrentBuild = log.contains("Building my-app-test-1 ${VERSION}")
				            // def changeLogSets = build.changeSets.items.msg
                            print build.number
                            print build.result
                            print currentStatus
                            print build.changeSets
                            echo "isCurrentBuild: ${isCurrentBuild}" 
                            if((build.result == null || currentStatus.contains("stable") || currentStatus.contains("normal")) && isCurrentBuild){
                                echo "INSIDE" 
                            }
                        }
                    }


                    
                    // currentBuildNum = BUILD_NUMBER
                    // echo "BUILD_NUMBER: ${BUILD_NUMBER}"
                    // echo "currentBuildNum: ${currentBuildNum}"
                    // while(currentBuildNum){
                    //     logUrl = "http://192.168.0.22:8080/job/test/${currentBuildNum}/consoleText"
                    //     def log = sh(script: 'curl -u ${JENKINS_AUTH} -k ' + logUrl, returnStdout: true).trim()
                    //     def temp = log.contains("my-app-test-1 ${VERSION}")
                    //     echo "SAME RELEASE: ${temp}"
                    //     if(temp){
                    //         sh "curl -u admin:password -s 'http://192.168.0.22:8080/job/test/${currentBuildNum}/api/xml?wrapper=changes&xpath=//changeSet//comment' >> x.txt"
                    //         currentBuildNum = currentBuildNum.toInteger() - 1
                    //         echo "currentBuildNum: ${currentBuildNum}"
                    //     } else {
                    //         currentBuildNum = 0
                    //     } 
                    // }    
                    // // sh 'cat x.txt'
                    // def varsFile = "x.txt"
                    // def content = readFile varsFile
                    // echo "content RELEASE: ${content}"
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
