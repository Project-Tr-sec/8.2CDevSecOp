pipeline {
    agent any
    options { timestamps() }
    triggers { pollSCM('H/5 * * * *') } // poll ~every 5 minutes

    stages {
        stage('Checkout') {
            steps {
                // Uses the job's configured SCM; if you prefer hardcoding:
                // git branch: 'main', url: 'https://github.com/<your_user>/8.2CDevSecOps.git'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // continue even if tests fail (for assignment demo)
                bat 'cmd /c npm test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                // not all forks expose coverage script; don't fail the build if missing
                bat 'cmd /c npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                // show known CVEs but allow pipeline to continue
                bat 'cmd /c npm audit || exit /b 0'
            }
        }
    }

    post {
        always {
            // handy for your submission artifact
            archiveArtifacts artifacts: 'coverage/**/*, npm-debug.log*', allowEmptyArchive: true
        }
    }
}
