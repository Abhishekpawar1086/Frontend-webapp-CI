def dockerImage
pipeline {
  agent any

  environment {
   GAR_REGISTRY = 'us-central1-docker.pkg.dev'
    PROJECT_ID = 'final-project-453412'
    FOLDER_NAME = 'docker-images'
    IMAGE_NAME = 'frontend-webapp'
    IMAGE_TAG = "${GAR_REGISTRY}/${PROJECT_ID}/${FOLDER_NAME}/${IMAGE_NAME}/${BUILD_NUMBER}"
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
    stage('Build Docker Image') {
            steps {
                 script {
                    dockerImage=docker.build("${IMAGE_TAG}")
        }
      }
    }
  stage('Authenticate with GCP') {
            steps {
                 script {
                   withCredentials([file(credentialsId: 'gcp-artifact-registry-key', variable: 'GCP_KEY_FILE')]) {
    sh '''
    gcloud auth activate-service-account --key-file=${GCP_KEY_FILE}
    gcloud auth configure-docker ${GAR_REGISTRY}
    '''

        }
      }
    }

  }


   stage('Push Docker Image') {
    steps {
        script {
            //docker.withRegistry("https://${GAR_REGISTRY}", 'gcp-artifact-registry-key') {
                //dockerImage.push()
              withDockerRegistry(credentialsId: 'gcp-artifact-registry-key', url: 'https://${GAR_REGISTRY}') {
              dockerImage.push()
            }
        }
    }
}
  }
}

