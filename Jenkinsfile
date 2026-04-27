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


        stage ('Secrets Scanning - GiLeaks') {
            steps {
                script {
                    sh '''
                        echo "Running GitLeaks Scan....."
                        gitleaks detect --source . \
                            --verbose \
                            --report-path ./gitleaks-report.json
                            --exit-code 0

                        echo "GitLeaks scan completed" 
                    '''
                }
            }
            post{
                always {
                    publishHTML([allowMissing: true, alwaysLinkToLastBuild: true, icon: '', keepAll: true, reportDir: '', reportFiles: 'gitleaks-report.json', reportName: 'GitLeaks Report', reportTitles: '', useWrapperFileDirectly: true])

                    archiveArtifacts allowEmptyArchive: true, artifacts: 'gitleaks-report.json'
                }
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