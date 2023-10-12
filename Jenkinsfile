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
                input message: 'Lanjutkan ke tahap Deploy?', parameters: [
                    booleanParam(description: 'Klik Proceed untuk melanjutkan dan Abord untuk mengakhiri', name: 'PROCEED')
                ]
            }
        }
        stage('Deploy') {
            when {
                expression {
                    params.PROCEED == true
                }
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
}
