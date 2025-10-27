pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://localhost:9000"
        SONAR_LOGIN = credentials('sqa_476a8d2282c8f4cf78ab4f9d9b1616194771c257')
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
