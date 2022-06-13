@NonCPS
def makeEnvAvailable() {
    // Goes through every environment variable and sets it again
    // This will populate 'downstreamBuild.getBuildVariables()'
    env.getEnvironment().each { k,v -> env.setProperty(k, v)  }
}
pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
     environment {
        TEST_VER="123"
        VIVARIUM="/tools/pancake"
    }
    stage ("Downstream") {
            steps {
                script {
                    def down = build job: "down"
                    echo down.getBuildVariables().toString()
                    echo down.buildVariables
                }
            }
        }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
                sh 'mvn -B -DskipTests clean package'
            }
        }
    }
}
