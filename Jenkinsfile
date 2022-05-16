pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '--volumes-from=jenkins -p 3000:3000'
        }
    }
    environment {
        CI = 'true'
        npm_config_cache = 'npm-cache'
    }
    stages {
        stage('Build') {
            steps {
                sh (script: 'npm install --loglevel verbose', returnStdout: true, returnStatus: true)
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
