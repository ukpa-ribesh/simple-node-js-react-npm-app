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
                        gitleaks detect source --source . --log-opts="-10" -r gitleaks-report.json -f json
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

        // stage ('Test') {
        //     steps {
        //         sh './jenkins/scripts/test.sh'
        //     }
        // }

        stage('Before Delivery') {
            steps {
                sh 'echo "Only for cicd1 Branch"'
                sh '''
                    echo "=== Installed Tools ===" && \
                            node --version && \
                            npm --version && \
                            git --version && \
                            docker --version && \
                            gitleaks version && \
                            sonar-scanner -v
                '''
            }
        }
    }
    post {
        always {
            sh 'echo This is cicd1 Branch'
}
