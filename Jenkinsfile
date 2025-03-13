pipeline {
  agent any

  environment {
    GAR-REGESTRY = 'us-central1-docker.pkg.dev'
    PROJECT-ID = 'final-project-453412'
    FOLDER-NAME = 'docker-images'
    IMAGE-NAME = 'frontend-webapp'
    IMAGE_TAG = "${GAR-REGESTRY}/${PROJECT-ID}/${FOLDER-NAME}/${IMAGE-NAME}/${BUILD_NUMBER}"
  }
  
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
           
           npm ci
           """
      }  
    } 

    /* stage("lint test") {
      steps {
        sh"""
           npm run lint
           """
      }  
    }*/
    stage("npm unit test") {
      steps {
        sh"""
           npm run test:unit
           """
      }  
    }
    stage('Build Docker Image') {
            steps {
                 script {
                    docker.build("${IMAGE_TAG}")
                }
            }
        }
  }
}

