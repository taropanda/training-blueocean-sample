pipeline {
  agent {
    docker {
      image 'bitwiseman/training-blueocean-sample'
      args '-u root -v $HOME/.m2:/root/.m2'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Backend": {
            sh './jenkins/test-backend.sh'
            junit '**/surefire-reports/**/*.xml'
            
          },
          "Frontend": {
            sh './jenkins/test-frontend.sh'
            junit '**/test-results/karma/*.xml'
            
          },
          "static": {
            sh './jenkins/test-static.sh'
            
          },
          "performance": {
            sh './jenkins/test-performance.sh'
            
          }
        )
      }
    }
    stage('Deploy to Staging') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }
    stage('Deploy to production') {
      steps {
        input(message: 'Deploy to production?', ok: '\'Fire away!\'')
        sh './jenkins/deploy.sh production'
        sh 'echo Notifying appropriate team members!'
      }
    }
  }
}