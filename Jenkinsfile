pipeline {
  agent any

  //environment {
  //}
  
  stages {
       stage('Clean Workspace') {
            steps {
                cleanWs() // Cleans the workspace
            }
        }
    stage("git-chekckout") {
      steps {
        git branch: 'main', credentialsId: 'Jenkins-token', url: 'https://github.com/Abhishekpawar1086/Frontend-webapp.git'
      }  
    }
    stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
          }
      }
     stage("npm test") {
      steps {
        sh"""
           npm init @eslint/config
           npm ci
           """
      }  
    } 

      stage("lint test") {
      steps {
        sh"""
           npm run lint
           """
      }  
    }
    stage("npm unit test") {
      steps {
        sh"""
           npm run test:unit
           """
      }  
    }
  }
}

