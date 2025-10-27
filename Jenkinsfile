pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://localhost:9000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Explicitly checkout the 'main' branch
                git branch: 'main', url: 'https://github.com/rizaldisakim/studi-kasus-5.1.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sqa_eed4ea4bdaecc3170bab700ef205728a2ecca31e', variable: 'SONAR_LOGIN')]) {
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
        success {
            echo "Pipeline selesai sukses ✅"
        }
        failure {
            echo "Pipeline gagal ❌"
        }
    }
}
