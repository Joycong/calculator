pipeline {
    agent {
        docker {
            image 'gradle:7.6.3-jdk17'
        }
    }
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage("Unit test") {
            steps {
                sh "./gradlew test"
            }
        }
    }
}
