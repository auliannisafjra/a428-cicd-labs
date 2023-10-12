pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan)'
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                script {
                    echo "Memberikan jeda 1 menit untuk menghentikan proses..."
                    sleep time: 60, unit: 'SECONDS'
                    echo "Sudah 1 menit, proses dihentikan."
                }
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
