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
                sh 'echo "Only for cicd1 Branch"'
            }
        }
    }
}