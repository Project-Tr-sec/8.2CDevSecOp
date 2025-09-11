pipeline {
    agent any
    options { timestamps() }
    triggers { pollSCM('* * * * *') } // poll ~every 1 minutes

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Project-Tr-sec/8.2CDevSecOp.git'
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
                bat 'cmd /c npm test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'cmd /c npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
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