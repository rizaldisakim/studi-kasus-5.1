pipeline {
    agent any

    environment {
        JAVA_HOME = "/opt/java/openjdk"
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
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

        stage('Verify Environment') {
            steps {
                // Verify Java and sonar-scanner exist
                sh '''
                    echo "Java version:"
                    java -version

                    if [ ! -f /var/jenkins_home/sonar-scanner-4.8.1.3023-linux/bin/sonar-scanner ]; then
                        echo "ERROR: sonar-scanner not found!"
                        exit 1
                    fi

                    ls -l /var/jenkins_home/sonar-scanner-4.8.1.3023-linux/bin/
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Use the Jenkins stored SonarQube token safely
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_LOGIN')]) {
                    sh '''
                        /var/jenkins_home/sonar-scanner-4.8.1.3023-linux/bin/sonar-scanner \
                            -Dsonar.projectKey=DemoProject \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=$SONAR_HOST_URL \
                            -Dsonar.login=$SONAR_LOGIN
                    '''
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
