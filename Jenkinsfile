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
                    dockerImage=docker.build("${IMAGE_TAG}")           // building image with image tag will leet jenkins know where to tranSfer this image (GAR) in building stage only for building and pushing only we add ENV variables
        }
      }
    }
  stage('Authenticate with GCP') {
            steps {
                 script {
                   withCredentials([file(credentialsId: 'gcp-artifact-registry-key', variable: 'GCP_KEY_FILE')]) {          //generated to authenticate with gcp using Pipline syntex and Service account JSON file
                    sh '''
                    gcloud auth activate-service-account --key-file=${GCP_KEY_FILE}                             
                     gcloud auth configure-docker ${GAR_REGISTRY}
                       '''
                      sh '''
                       docker login -u _json_key -p "$(cat '''+GCP_KEY_FILE+''')" https://'''+GAR_REGISTRY+'''
                       docker push '''+IMAGE_TAG+'''
                       
                        '''
        }  //from this sh ''' to the end you need to copy hte code from some were 
      }
    }
 }
   /* stage('Push Docker Image') {
    steps {
        script {
  sh '''
                       docker login -u _json_key -p "$(cat '''+GCP_KEY_FILE+''')" https://'''+GAR_REGISTRY+'''
                       docker push '''+IMAGE_TAG+'''
                       
                        '''
          
        }
      }
    } */
  }
}


