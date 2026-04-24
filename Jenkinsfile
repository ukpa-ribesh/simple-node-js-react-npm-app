pipeline {

    agent any

    tools {
        nodejs 'nodejs-25-9-0'
    }

    stages {
        stage('Build NPM') {
            steps {
                sh 'npm install'
            }
        }

        stage ('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('Before Delivery') {
            steps {
                sh 'echo "Pipeline is complete!!"'
            }
        }

        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the website? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }

        
    }
}