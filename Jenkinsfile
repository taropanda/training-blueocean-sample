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
  }
}