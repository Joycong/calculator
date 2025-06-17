pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/leszko/calculator.git'
            }
        }

        stage('Compile') {
            steps {
                sh './gradlew compileJava'
            }
        }

        stage('Unit test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t leszko/calculator .'
            }
        }

        stage('Run container') {
            steps {
                sh 'docker run -d --rm --name calculator -p 8765:8080 leszko/calculator'
            }
        }

        stage('Acceptance Test') {
            steps {
                // 핵심: 테스트 URL 변경
                sh './gradlew acceptanceTest -Dcalculator.url=http://host.docker.internal:8765'
            }
        }

        stage('Stop container') {
            steps {
                sh 'docker stop calculator || true'
            }
        }
    }
}
