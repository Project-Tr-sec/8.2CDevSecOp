pipeline {
  agent any

  // If you have "Global Tool Configuration" -> NodeJS tool named "NodeJS_20", keep this.
  // Otherwise you can remove the tools block and ensure node/npm are on PATH.
  tools {
    nodejs 'NodeJS_20'       // <-- rename/remove if you don't use a NodeJS tool
  }

  options {
    timestamps()
  }

  // Poll SCM every minute
  triggers {
    pollSCM('* * * * *')
  }

  stages {
    stage('Checkout') {
      steps {
        // Corrected syntax and repo URL (note the 's' in DevSecOps)
        git branch: 'main', url: 'https://github.com/Project-Tr-sec/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        // Use npm ci if you have a package-lock.json; fallback to install otherwise
        script {
          if (fileExists('package-lock.json')) {
            bat 'npm ci'
          } else {
            bat 'npm install'
          }
        }
      }
    }

    stage('Run Tests') {
      steps {
        // Let pipeline continue even if tests fail (remove "|| exit /b 0" to fail on test errors)
        bat 'npm test || exit /b 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        // Assumes a "coverage" script exists in package.json
        bat 'npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        // Show known CVEs but do not fail the build
        bat 'npm audit || exit /b 0'
      }
    }
  }
}
