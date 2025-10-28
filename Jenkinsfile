pipeline {
    agent any

    environment {
        SONAR_HOST_URL = "http://sonarqube:9000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clean checkout to avoid "not in a git directory" errors
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [[$class: 'CleanCheckout']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/rizaldisakim/studi-kasus-5.1.git'
                    ]]
                ])
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_LOGIN')]) {
                    sh '''#!/bin/bash
                        echo "Running SonarQube analysis via Docker..."
                        docker run --rm \
                            -e SONAR_HOST_URL=${SONAR_HOST_URL} \
                            -e SONAR_LOGIN=${SONAR_LOGIN} \
                            -v ${WORKSPACE}:/usr/src \
                            sonarsource/sonar-scanner-cli \
                            -Dsonar.projectKey=DemoProject \
                            -Dsonar.sources=/usr/src
                    '''
                }
            }
        }
    }

    post {
        success { echo "Pipeline selesai sukses ✅" }
        failure { echo "Pipeline gagal ❌" }
    }
}
