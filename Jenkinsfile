pipeline {
    agent {
        docker {
            image 'node:16-alpine'
            args "-p 3000:3000 -v ${windowsPath(WORKSPACE)}:/app -w /app"
        }
    }
    environment {
        CI = 'true'
        WORKSPACE = "$PWD"
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'mytestimage:latest'
                }
            }
            steps {
                sh 'npm test'
            }
        }
        stage('Deliver') {
            agent {
                docker {
                    image 'mydeliveryimage:latest'
                }
            }
            steps {
                sh 'deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh 'kill.sh'
            }
        }
    }
}
