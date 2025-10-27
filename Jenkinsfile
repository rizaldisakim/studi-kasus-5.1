pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://localhost:9000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/rizaldisakim/studi-kasus-5.1.git'
                    ]]
                ])
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_LOGIN')]) {
                    sh """
                    sonar-scanner \
                        -Dsonar.projectKey=DemoProject \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${SONAR_LOGIN}
                    """
                }
            }
        }
    }

    post {
        success { echo "Pipeline selesai sukses ✅" }
        failure { echo "Pipeline gagal ❌" }
    }
}
