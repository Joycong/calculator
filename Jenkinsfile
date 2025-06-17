pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Joycong/calculator.git'
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
                sh 'docker build -t calculator .'
            }
        }

        stage('Run container') {
            steps {
                // 네트워크 생성 (이미 존재해도 무시)
                sh 'docker network create jenkins-net || true'
                // calculator 컨테이너를 같은 네트워크에서 실행
                sh 'docker run -d --rm --name calculator --network jenkins-net calculator'
                // 실행 대기
                sh 'sleep 5'
            }
        }

        stage('Acceptance Test') {
            steps {
                // 도메인 이름으로 접근
                sh './gradlew acceptanceTest -Dcalculator.url=http://calculator:8080'
            }
        }

        stage('Stop container') {
            steps {
                // --rm으로 실행했기 때문에 생략 가능
                sh 'echo "Container was set to auto-remove."'
            }
        }
    }
}
