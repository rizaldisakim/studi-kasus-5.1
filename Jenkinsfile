pipeline {
    agent any

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
                        docker run --rm \
                        -e SONAR_HOST_URL=http://sonarqube:9000 \
                        -e SONAR_LOGIN=$SONAR_LOGIN \
                        -v \$(pwd):/usr/src \
                        sonarsource/sonar-scanner-cli \
                        -Dsonar.projectKey=DemoProject \
                        -Dsonar.sources=/usr/src
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

