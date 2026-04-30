pipeline {

    agent {
        docker {
            image 'rentalbux-ci-agent:latest'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

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
                        BASE=$(git rev-parse HEAD~5)
                        gitleaks detect --source . --commit-from $BASE --commit-to HEAD
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