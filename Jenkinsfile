pipeline {
  agent any
  
  environment {
    PROJECT_ID = "gps-intregated"
    SERVICE_NAME = "test"
    REGION = "us-central1"
    IMAGE_NAME = "myimage"
    REGISTRY_HOSTNAME = "gcr.io"
  }
  
  stages {
    stage('Build and push Docker image') {
      steps {
        // Authenticate with Google Cloud using service account key
        withCredentials([file(credentialsId: 'test', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
         
            bat "cmd /c gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
            bat "cmd /c  gcloud components install docker-credential-gcr"
            bat "cmd /c gcloud auth configure-docker --quiet"
         
        }
        
        // Build Docker image and tag it
       bat "cmd /c docker build -t ${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME}:${TAG} ."
        
        // Push Docker image to Google Artifact Registry
        bat "cmd /c docker push ${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME}:${TAG}"
      }
    }
    
    stage('Deploy to Cloud Run') {
      steps {
        // Deploy the Docker image to Cloud Run
      bat """ cmd /c
          gcloud run deploy ${SERVICE_NAME} \
            --image=${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME}:${TAG} \
            --platform=managed \
            --region=${REGION} \
            --allow-unauthenticated
        """
      }
    }
  }
}
