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

        // stage ('Test') {
        //     steps {
        //         sh './jenkins/scripts/test.sh'
        //     }
        // }

        stage('Before Delivery') {
            steps {
                sh 'echo "Only for cicd Branch"'
            }
        }
    }
    post {
        always {
            sh 'echo this is cicd branch done manually'
            sh 'echo Just testing'
        }
    }
}
