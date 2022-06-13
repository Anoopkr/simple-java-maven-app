@NonCPS
def makeEnvAvailable() {
    // Goes through every environment variable and sets it again
    // This will populate 'downstreamBuild.getBuildVariables()'
    env.getEnvironment().each { k,v -> env.setProperty(k, v)  }
}
def listener = Jenkins.get()
    .getItemByFullName(env.JOB_NAME)
    .getBuildByNumber(Integer.parseInt(env.BUILD_NUMBER))
    .getListener()
pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
     environment {
        TEST_VER="123"
        VIVARIUM="/tools/pancake"
    }  
    stages {
        
        stage('build') {
            steps {
                echo build.getEnvironment(listener)
                sh 'mvn --version'
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}
