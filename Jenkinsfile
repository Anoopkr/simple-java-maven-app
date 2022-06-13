pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
     environment {
        TEST_VER="123"
        VIVARIUM="/tools/pancake"
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
