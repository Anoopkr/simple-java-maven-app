pipeline {
  agent {
    docker {
      image 'maven:3.8.4-openjdk-11-slim'
    }
  }
  environment {
    JENKINS_AUTH = credentials('jenkins_cred')
    VERSION = readMavenPom().getVersion()
  }

  stages {

    stage('build') {
      steps {
        script {
          echo "VERSION: ${VERSION}"
          sh 'mvn -B -DskipTests clean package'
            def changeLog = ""
          Jenkins.instance.getItem("test").each {

            def jobBuilds = it.getBuilds()

            for(def build : jobBuilds) { // this may need some tweaking depending on types
                  def currentStatus = build.buildStatusSummary.message
                  def log = build.log              
                  // def changeLogSets = build.changeSets.items.msg
                  if ((build.result == null || currentStatus.contains("stable") || currentStatus.contains("normal"))) {
                      print build.number
        		      def isCurrentBuild = log.contains("Building my-app-test-1 ${VERSION}")
        		      if(isCurrentBuild){
        			      build.changeSets.items.each {
        				  item ->
        				    print item.msg
        				    changeLog = changeLog + "\n" + item.msg
        				}
        		      } else {
        			      
        			      print "EXIT"
        			      break;
        			      
        		      }
                }
            }
            echo "changeLog: ${changeLog}"
          }
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
