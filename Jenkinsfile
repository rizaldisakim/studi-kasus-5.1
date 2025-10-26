pipeline {
    agent any

    environment {
        SONAR_TOKEN = 'sqa_8f7503988dc85ab2a7d9c3939d609b9205823e30'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/rizaldisakim/studi-kasus-5.1.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Build step - ganti sesuai proyekmu"'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                sh '''
                    sonar-scanner \
                        -Dsonar.projectKey=MyProject \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=${sqa_8f7503988dc85ab2a7d9c3939d609b9205823e30}
                '''
            }
        }

        stage('Dependency Check') {
            steps {
                sh 'dependency-check.sh --project MyProject --format HTML --out dependency-check-report'
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML([
                    reportDir: 'dependency-check-report',
                    reportFiles: 'index.html',
                    reportName: 'Dependency Check Report'
                ])
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
