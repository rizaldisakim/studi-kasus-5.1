pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://localhost:9000"
        SONAR_LOGIN = credentials('sqa_eed4ea4bdaecc3170bab700ef205728a2ecca31e')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/rizaldisakim/studi-kasus-5.1.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
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

    post {
        success {
            echo "Pipeline selesai sukses ✅"
        }
        failure {
            echo "Pipeline gagal ❌"
        }
    }
}
