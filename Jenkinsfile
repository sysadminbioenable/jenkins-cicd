pipeline {
  agent any
  
  environment {
    PROJECT_ID = "nisarg-cludrun"
    SERVICE_NAME = "test"
    REGION = "us-central1"
    IMAGE_NAME = "myimage"
    REGISTRY_HOSTNAME = "asia.gcr.io"
  }
  
  stages {
    stage('Build and push Docker image') {
      steps {
        // Authenticate with Google Cloud using service account key
        withCredentials([file(credentialsId: 'test', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
         
            bat "cmd /c gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}"
           
            bat "cmd /c gcloud auth configure-docker --quiet"
            bat "cmd /c gcloud auth configure-docker asia.gcr.io"
         
        }
        
        // Build Docker image and tag it
       bat "cmd /c docker build -t ${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME} ."
        
        // Push Docker image to Google Artifact Registry
        bat "cmd /c docker push ${REGISTRY_HOSTNAME}/${PROJECT_ID}/${IMAGE_NAME}"
      }
    }
    
    stage('Deploy to Cloud Run') {
      steps {
        // Deploy the Docker image to Cloud Run
      bat """ cmd /c
          gcloud run deploy ${SERVICE_NAME} \
            --image=${REGISTRY_HOSTNAME}/nisarg-cludrun/${IMAGE_NAME} \
            --platform=managed \
            --region=${REGION} \
            --project="nisarg-cludrun"
            --allow-unauthenticated
        """
      }
    }
  }
}
