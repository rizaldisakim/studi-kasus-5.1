pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://sonarqube:9000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout main branch
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
                // Use the Jenkins stored SonarQube token
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_LOGIN')]) {
                    sh """
                    /var/jenkins_home/sonar-scanner-4.8.1.3023-linux/bin/sonar-scanner \
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
