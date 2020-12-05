pipeline {
  agent any
  options {
    // Keep 10 most recent build
    buildDiscarder(logRotatior(numToKeepStr:'10'))
  }
  stages{
    stage('Install') {
      steps {
        // Install required gems
        sh 'bundle install'
      }
    }
    stage('Build') {
      steps {
        // Build
        sh 'bundle exec rake build'
        
        // Archive the build artifacts
        archive includes: 'pkg/*.gem'
      }
    }
    stage('Test') {
      steps {
        // Run test with coverage
        sh 'bundle exec rake test'

        // Publish HTML
        publishHTML target: [
          allowMissing: false,
          alwaysLinkToLastBuild: false,
          keepAll: true,
          reportDir: 'coverage',
          reportFiles: 'index.html',
          reportName: 'RCov Report'
        ]
      }
    }
  }
}