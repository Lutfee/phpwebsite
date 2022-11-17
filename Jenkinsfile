pipeline {
  agent any
  stages {
    stage('Stopping Previous Build') {
      steps {
        cancelPreviousBuilds()
      }
    }
    
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
      
    stage('OWASP DependencyCheck') {
      steps {
        dependencyCheck additionalArguments: '--format HTML --format XML ' , odcInstallation: 'Default'
      }
    }
    stage('Docker compose') {
      steps {
        sh "docker compose -f docker-compose.yml up --build"
      }
    }
  }
  
  post {
    success {
      dependencyCheckPublisher pattern: 'dependency-check-report.xml'
    }
  }
}
